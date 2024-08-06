global function MpWeaponGrenadeBangalore_Init
global function OnWeaponActivate_weapon_grenade_bangalore
global function OnProjectileCollision_weapon_grenade_bangalore
global function OnProjectileIgnite_weapon_grenade_bangalore
global function OnWeaponTossReleaseAnimEvent_weapon_grenade_bangalore

global function OnClientAnimEvent_weapon_grenade_bangalore

global function BangSmoke_IsPlayerHighlighted



global const string BANGALORE_SMOKESCREEN_SCRIPTNAME = "bangalore_smokescreen"

const asset FX_SMOKESCREEN_BANGALORE = $"P_smokescreen_FD"
const asset FX_SMOKEGRENADE_TRAIL = $"P_SmokeScreen_FD_trail"
const asset BANGALORE_SMOKE_MODEL = $"mdl/weapons/grenades/w_bangalore_canister_gas_projectile.rmdl"

const float BANGALORE_SMOKE_MAX_SAFE_SPAWN_PELLET_DISTANCE = 32.0
const float BANGALORE_SMOKE_DURATION = 8.6
const float BANGALORE_SMOKE_MIN_EXPLODE_DIST_SQR = 512 * 512
const float BANGALORE_SMOKE_DISPERSAL_TIME = 3.0
const float BANGALORE_TACTICAL_AGAIN_TIME = 4.0

const float UPGRADE_BANGALORE_SMOKE_HP_REGEN_RATE_PER_SEC = 3.5
const float UPGRADE_BANGALORE_SMOKE_HP_REGEN_DELAY = 1.0


const bool BANGALORE_SMOKE_EXPLOSIONS = false
const asset SMOKE_SCREEN_FX = $"P_screen_smoke_bangalore_FP"

const asset FX_MUZZLE_FLASH_FP = $"P_wpn_mflash_bang_rocket_FP"
const asset FX_MUZZLE_FLASH_3P = $"P_wpn_mflash_bang_rocket"

const string BANGALORE_SMOKE_FX_TABLE = "exp_creeping_barrage"

struct
{








		int colorCorrectionGas


		float highlightFalloffDistance = 10 * METERS_TO_INCHES
		float highlightDistance = 20 * METERS_TO_INCHES

		table< entity, table<entity, bool> > highlightedPlayersInSmoke


	int smokeGasScreenFxId
} file

void function MpWeaponGrenadeBangalore_Init()
{
	PrecacheModel( BANGALORE_SMOKE_MODEL )
	PrecacheParticleSystem( FX_SMOKESCREEN_BANGALORE )
	PrecacheParticleSystem( FX_SMOKEGRENADE_TRAIL )
	PrecacheParticleSystem( FX_MUZZLE_FLASH_FP )
	PrecacheParticleSystem( FX_MUZZLE_FLASH_3P )
	PrecacheScriptString( BANGALORE_SMOKESCREEN_SCRIPTNAME )

	PrecacheImpactEffectTable( BANGALORE_SMOKE_FX_TABLE )

	file.smokeGasScreenFxId = PrecacheParticleSystem( SMOKE_SCREEN_FX )










		RegisterSignal( "stop_smokescreen_screen_fx" )

		StatusEffect_RegisterEnabledCallback( eStatusEffect.smokescreen, BangaloreSmokescreenEffectEnabled )
		StatusEffect_RegisterDisabledCallback( eStatusEffect.smokescreen, BangaloreSmokescreenEffectDisabled )

		file.colorCorrectionGas = ColorCorrection_Register( "materials/correction/smoke_cloud.raw_hdr" )

}



































void function OnWeaponActivate_weapon_grenade_bangalore( entity weapon )
{
}


var function OnWeaponTossReleaseAnimEvent_weapon_grenade_bangalore( entity weapon, WeaponPrimaryAttackParams attackParams )
{









	return Grenade_OnWeaponTossReleaseAnimEvent( weapon, attackParams )
}


void function OnProjectileCollision_weapon_grenade_bangalore( entity projectile, vector pos, vector normal, entity hitEnt, int hitBox, bool isCritical, bool isPassthrough )
{
	entity player = projectile.GetOwner()
	if ( hitEnt == player )
		return

	if ( projectile.GrenadeHasIgnited() )
		return





	if ( LengthSqr( normal ) < 0.01 ) 
		normal = AnglesToForward( VectorToAngles( projectile.GetVelocity() ) )





	DeployableCollisionParams collisionParams
	collisionParams.pos = pos
	collisionParams.normal = normal
	collisionParams.hitEnt = hitEnt
	collisionParams.hitBox = hitBox
	collisionParams.isCritical = isCritical
	collisionParams.highDetailTrace = true

	projectile.SetModel( $"mdl/dev/empty_model.rmdl" )
	bool result = PlantStickyEntity( projectile, collisionParams, normal )

	projectile.GrenadeIgnite()
	projectile.SetDoesExplode( false )
}

void function OnProjectileIgnite_weapon_grenade_bangalore( entity projectile )
{









































































}

float function Bangalore_GetSmokeDuration( entity player )
{
	float duration = GetCurrentPlaylistVarFloat( "bangalore_smoke_lifetime", BANGALORE_SMOKE_DURATION )










		if( PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_TWO ) ) 
			duration += 3


	return duration
}

bool function Bangalore_ShouldSmokesExplode( entity player )
{





	return BANGALORE_SMOKE_EXPLOSIONS
}































































































































































































































































void function OnClientAnimEvent_weapon_grenade_bangalore( entity weapon, string name )
{
	GlobalClientEventHandler( weapon, name )

	if ( name == "muzzle_flash" )
	{
		weapon.PlayWeaponEffect( FX_MUZZLE_FLASH_FP, FX_MUZZLE_FLASH_3P, "muzzle_flash" )
	}
}

void function BangaloreSmokescreenEffectDisabled( entity ent, int statusEffect, bool actuallyChanged )
{
	if ( ent != GetLocalViewPlayer() )
		return

	ent.Signal( "GasCloud_StopColorCorrection" )
	ent.Signal( "stop_smokescreen_screen_fx" )

	Chroma_EndSmokescreenEffect()
}

void function BangaloreSmokescreenEffectEnabled( entity ent, int statusEffect, bool actuallyChanged )
{
	if ( ent != GetLocalViewPlayer() )
		return

	entity viewPlayer = ent
	viewPlayer.Signal( "stop_smokescreen_screen_fx" )

	thread UpdatePlayerScreenColorCorrection( viewPlayer, statusEffect, file.colorCorrectionGas )


	if ( BangSmokeHighlightsEnabled() )
	{
		thread SmokeHighlight_Thread( ent )
	}


	if ( !viewPlayer.IsTitan() )
	{
		int fxHandle = StartParticleEffectOnEntityWithPos( viewPlayer, file.smokeGasScreenFxId, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID, viewPlayer.EyePosition(), <0, 0, 0> )
		EffectSetIsWithCockpit( fxHandle, true )

		Chroma_StartSmokescreenEffect()

		thread BangaloreSmokescreenEffectThread( viewPlayer, fxHandle, statusEffect )
	}
}

void function BangaloreSmokescreenEffectThread( entity ent, int fxHandle, int statusEffect )
{
	EndSignal( ent, "OnDeath" )
	EndSignal( ent, "stop_smokescreen_screen_fx" )

	OnThreadEnd(
		function() : ( fxHandle )
		{
			if ( !EffectDoesExist( fxHandle ) )
				return

			EffectStop( fxHandle, false, true )
		}
	)

	while( true )
	{
		float severity = StatusEffect_GetSeverity( ent, statusEffect )
		

		if ( !EffectDoesExist( fxHandle ) )
			break

		EffectSetControlPointVector( fxHandle, 1, <severity, 999, 0> )
		WaitFrame()
	}
}


bool function BangSmokeHighlightsEnabled()
{





	return GetCurrentPlaylistVarBool( "bangalore_smoke_highlight", true )
}

void function SmokeHighlight_Thread( entity player )
{
	EndSignal( player, "OnDestroy", "OnDeath", "stop_smokescreen_screen_fx" )

	table< entity, bool > highlightedPlayers
	file.highlightedPlayersInSmoke[player] <- highlightedPlayers

	OnThreadEnd(
		function() : ( player, highlightedPlayers )
		{
			delete file.highlightedPlayersInSmoke[player]

			foreach ( entity otherPlayer, bool isHightlighted in highlightedPlayers )
			{
				ManageHighlightEntity( otherPlayer )
			}
		}
	)

	const float effectMin = 0.6
	const float effectMax = 0.2
	int contextId = HighlightContext_GetId( "smoke_highlight_blockscan" )

	while(true)
	{
		array< entity > playersToHighlight
		array< entity > playersToNotHighlight

		float effectStrength = StatusEffect_GetSeverity( player, eStatusEffect.smokescreen )
		float alphaFromStatusEffect = GraphCapped( effectStrength, effectMin, effectMax, 1.0, 0.0 )

		HighlightContext_SetParam( contextId, 1, <file.highlightFalloffDistance, file.highlightDistance, alphaFromStatusEffect> )

		BangSmoke_GetPlayersToHighlight( player, playersToHighlight, playersToNotHighlight )

		foreach( entity otherPlayer in playersToHighlight )
		{
			if ( !( otherPlayer in highlightedPlayers ) )
			{
				highlightedPlayers[ otherPlayer ] <- false
			}

			if ( highlightedPlayers[otherPlayer] )
			{
				continue
			}

			highlightedPlayers[otherPlayer] = true

			ManageHighlightEntity( otherPlayer )
		}

		foreach( entity otherPlayer in playersToNotHighlight )
		{
			if ( otherPlayer in highlightedPlayers )
			{
				if ( !highlightedPlayers[otherPlayer] )
					continue

				highlightedPlayers[otherPlayer] = false

				ManageHighlightEntity( otherPlayer )
			}
		}

		WaitFrame()
	}

}

void function BangSmoke_GetPlayersToHighlight( entity player,
		array<entity> outPlayersToHighlight, array<entity> outPlayersToNotHighlight )
{
	int playerTeam = player.GetTeam()

	vector visibilityCheckPoint = player.EyePosition()
	float maxSqrDist = file.highlightDistance * file.highlightDistance

	array<entity> allTargets = GetPlayerArray_Alive()
	allTargets.extend( GetEntArrayByScriptName( DECOY_SCRIPTNAME ) )
	allTargets.extend( GetEntArrayByScriptName( CONTROLLED_DECOY_SCRIPTNAME ) )

	TraceResults results
	foreach ( entity otherPlayer in allTargets )
	{
		if ( otherPlayer == player )
			continue

		if ( !IsEnemyTeam( playerTeam, otherPlayer.GetTeam() ) )
			continue

		if ( StatusEffect_GetSeverity( otherPlayer, eStatusEffect.smokescreen ) >= 0.2 )
		{
			vector otherPlayerEyePos = otherPlayer.EyePosition()

			if ( DistanceSqr( visibilityCheckPoint, otherPlayerEyePos ) < maxSqrDist )
			{
				results = TraceLine( visibilityCheckPoint, otherPlayerEyePos, allTargets, TRACE_MASK_SOLID, TRACE_COLLISION_GROUP_NONE )
				if ( results.fraction == 1.0 )
				{
					outPlayersToHighlight.append( otherPlayer )
					continue
				}
			}
		}

		outPlayersToNotHighlight.append( otherPlayer )
	}
}

bool function BangSmoke_IsPlayerHighlighted( entity player, entity otherPlayer )
{
	if ( !BangSmokeHighlightsEnabled() )
		return false

	if ( !(player in file.highlightedPlayersInSmoke) )
		return false

	if ( !(otherPlayer in file.highlightedPlayersInSmoke[player]) )
		return false

	if ( !file.highlightedPlayersInSmoke[player][otherPlayer] )
		return false

	return true
}



