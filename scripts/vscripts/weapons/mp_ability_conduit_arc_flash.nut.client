global function MpAbilityConduitArcFlash_Init

global function OnWeaponOwnerChanged_ability_conduit_arc_flash
global function OnWeaponActivate_ability_conduit_arc_flash
global function OnWeaponDeactivate_ability_conduit_arc_flash
global function OnWeaponReadyToFire_ability_conduit_arc_flash
global function OnWeaponAttemptOffhandSwitch_ability_conduit_arc_flash
global function OnWeaponPrimaryAttack_ability_conduit_arc_flash
global function OnWeaponPrimaryAttackAnimEvent_ability_conduit_arc_flash

global function GetArcFlashRangeSqr
global function GetArcFlashState
global function Conduit_GetArrayOfPossibleAllies










global function GetConduitShieldRui


const float TARGETING_CONE_DOT = DOT_50DEGREE

const float ARC_FLASH_INCLUSIVE_RANGE = 5 * METERS_TO_INCHES
const float ARC_FLASH_MIN_RANGE = 5 * METERS_TO_INCHES
const float ARC_FLASH_MIN_RANGE_SQR = ARC_FLASH_MIN_RANGE * ARC_FLASH_MIN_RANGE
global const float ARC_FLASH_MAX_RANGE = 50 * METERS_TO_INCHES
const float LOS_MAX_TIME_MISSING = 0.5

const float ARC_FLASH_REGEN_DURATION = 9
const float ARC_FLASH_REGEN_SEVERITY = 1.0
const float ARC_FLASH_TEMPSHIELD_DURATION = 20.0
const float ARC_FLASH_TEMPSHIELD_SEVERITY = 0.5
const float ARC_FLASH_REGEN_INTERVAL = 0.2
const int ARC_FLASH_REGEN_SHIELD_PER_FRAME = 3
const float ARC_FLASH_REGEN_DAMAGE_DELAY = 1.0
const float ARC_FLASH_REGEN_SELF_MULTIPLIER = 0.67
const int ARC_FLASH_DECAY_RATE = 2
const float ARC_FLASH_DECAY_SEVERITY = 0.1
global const int MAX_TEMPSHIELD = 125


const string ARC_FLASH_NAME = "mp_ability_conduit_arc_flash"
const string ARC_FLASH_TARGET_FAIL_GENERIC = "No targets found"
const string ARC_FLASH_TARGET_FAIL_NO_SHIELDS = "Not enough shields or health"
const string ARC_FLASH_TARGET_FAIL_LOS = "Line of sight obstructed"
const string ARC_FLASH_TARGET_FAIL_FACING = "No ally in view"
const string ARC_FLASH_TARGET_FAIL_RANGE = "Out of range"
const string ARC_FLASH_TARGET_FAIL_FULL_SHIELDS = "Friendly shields full"
const string ARC_FLASH_TARGET_SUCCESS_USING_HEALTH = "Using health"



const string SOUND_ARC_FLASH_BEAM_1P = "Conduit_Tac_Fire_1p"
const string SOUND_ARC_FLASH_BEAM_3P	= "Conduit_Tac_Fire_3p"

const string SOUND_ARC_FLASH_LESSER_BEAM_1P = "Conduit_Tac_FireSelf_1p"
const string SOUND_ARC_FLASH_LESSER_BEAM_3P	= "Conduit_Tac_FireSelf_3p"

const string SOUND_ARC_FLASH_RECEIVE_1P = "Conduit_Tac_ImpactTeam_1p" 
const string SOUND_ARC_FLASH_RECEIVE_3P = "Conduit_Tac_ImpactTeam_3p" 

const string SOUND_TEMPSHIELD_CHARGE_1P = "Conduit_Tac_Healing_Loop_1p"
const string SOUND_TEMPSHIELD_CHARGE_3P = "Conduit_Tac_Healing_Loop_3p"
const string SOUND_TEMPSHIELD_CHARGE_END_1P = "Conduit_Tac_Healing_End_1p"
const string SOUND_TEMPSHIELD_CHARGE_END_3P = "Conduit_Tac_Healing_End_3p"
const string SOUND_TEMPSHIELD_CHARGE_FINISHING_1P = "Conduit_Tac_Healing_Finishing_1p"
const float SOUND_TEMPSHIELD_CHARGE_FINISHING_DURATION = 4.0
const string SOUND_TEMPSHIELD_SHIELD_WARNING_1P = "Conduit_Tac_Shields_Ending_Warning_1p"
const float SOUND_TEMPSHIELD_SHIELD_WARNING_DURATION = 2.0


const asset FX_TAC_MUZZLE_FLASH = $"P_con_tac_MuzzleFX"
const asset FX_TAC_BEAM = $"P_con_tac_energyRope"
const asset FX_TEMPSHIELD_1P = $"P_con_tac_buff_1p"
const asset FX_TEMPSHIELD_3P = $"P_con_tac_regen_test"
const asset FX_TEMPSHIELD_HIT_3P = $"P_con_tac_hitFX"
const string MUZZLE_ATTACH = "attach_l_drone_arm_b"

const asset DEBUG_SPHERE_SOFT_FX = $"debug_sphere_soft"
const asset DEBUG_SPHERE_ADD_EDGE_FX = $"debug_sphere_add_edge"


global const string CONDUIT_ARC_FLASH_BEST_TARGET_NETVAR = "conduit_arc_flash_bestTarget"
global const string TEMPSHIELD_ACTIVE_NETVAR = "tempshields_active"

const bool ARC_FLASH_DEBUG = false

enum eLockResult
{
	FAILED_GENERIC,
	FAILED_NOT_FACING_ALLY,
	FAILED_NO_LOS_TO_ALLY,
	FAILED_OUT_OF_RANGE,
	FAILED_ALREADY_FULL,
	THRESHOLD,
	SUCCESS,
}

global enum eArcFlashState
{
	NONE,
	CHARGE,
	ACTIVE,
	DECAY,
	COUNT
}


#if DEV
array<string> sArcFlashStateStrings =
[
	"NONE"
	"CHARGE",
	"ACTIVE",
	"DECAY"
]
#endif



enum eShieldState
{
	INVALID,
	FULL,
	HIGH,
	LOW,
	CRITICAL,
}


struct
{

		var shieldsRepairingRui = null


	table< entity, array<entity> > trackedAllys





} file

void function MpAbilityConduitArcFlash_Init()
{
	PrecacheParticleSystem( FX_TAC_MUZZLE_FLASH )
	PrecacheParticleSystem( FX_TAC_BEAM )
	PrecacheParticleSystem( FX_TEMPSHIELD_1P )
	PrecacheParticleSystem( FX_TEMPSHIELD_3P )
	PrecacheParticleSystem( FX_TEMPSHIELD_HIT_3P )
	PrecacheParticleSystem( DEBUG_SPHERE_SOFT_FX )
	PrecacheParticleSystem( DEBUG_SPHERE_ADD_EDGE_FX )

	RegisterSignal( "TargetingStop" )
	RegisterSignal( "RefreshTempshield" )

	RegisterNetworkedVariable( CONDUIT_ARC_FLASH_BEST_TARGET_NETVAR, SNDC_PLAYER_EXCLUSIVE, SNVT_ENTITY )
	RegisterNetworkedVariable( TEMPSHIELD_ACTIVE_NETVAR, SNDC_PLAYER_GLOBAL, SNVT_BOOL, false )





#if DEV
	Assert( eArcFlashState.COUNT == sArcFlashStateStrings.len(), "Must define a string for each state." )
#endif


		StatusEffect_RegisterEnabledCallback( eStatusEffect.shields_repairing, ArcFlash_StartShieldsRepairing )
		StatusEffect_RegisterDisabledCallback( eStatusEffect.shields_repairing, ArcFlash_StopShieldsRepairing )

		RegisterSignal( "ArcFlash_EndShieldsRepairing" )

}


float function GetArcFlashUpgradedRangeScaler()
{
	return GetCurrentPlaylistVarFloat( "upgrade_arc_flash_range_scaler", 1.2 ) 
}


float function GetArcFlashRange( entity player )
{
	float result = ARC_FLASH_MAX_RANGE


	if( PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_ONE ) ) 
	{
		result *= GetArcFlashUpgradedRangeScaler()
	}


	return result
}


float function TempshieldRegen_GetExtraChargeRegenScaler()
{
	return GetCurrentPlaylistVarFloat( "upgrade_arc_flash_exta_charge_regen_scaler", .5 ) 
}


float function GetArcFlashDuration( entity player, int state )
{
	float result = 1

	switch ( state )
	{
		case eArcFlashState.CHARGE:
			result = ARC_FLASH_REGEN_DURATION

			if( PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_TWO ) ) 
			{
				result *= TempshieldRegen_GetExtraChargeRegenScaler()
			}

			break
		case eArcFlashState.ACTIVE:
			result = ARC_FLASH_TEMPSHIELD_DURATION
			break
	}

	return result
}

float function GetArcFlashRangeSqr( entity player )
{
	float range = GetArcFlashRange( player )
	return range * range
}

void function OnWeaponOwnerChanged_ability_conduit_arc_flash( entity weapon, WeaponOwnerChangedParams changeParams )
{

	if ( weapon.GetOwner() == GetLocalClientPlayer() )

	{
		array<entity> allyList
		file.trackedAllys[ weapon.GetOwner() ] <- allyList
		if ( IsValid( changeParams.oldOwner ) )
		{
			 changeParams.oldOwner.Signal("TargetingStop")
		}

		if ( IsValid( changeParams.newOwner ) )
		{
			




			thread TargetingHUD_Thread( changeParams.newOwner )

		}
	}

}

bool function OnWeaponAttemptOffhandSwitch_ability_conduit_arc_flash( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	if ( !IsValid( player ) )
		return false

	return true
}

void function OnWeaponActivate_ability_conduit_arc_flash( entity weapon )
{









}

void function OnWeaponDeactivate_ability_conduit_arc_flash( entity weapon )
{



















}

void function OnWeaponReadyToFire_ability_conduit_arc_flash( entity weapon )
{











}




































































var function OnWeaponPrimaryAttack_ability_conduit_arc_flash( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	return true 
}


var function OnWeaponPrimaryAttackAnimEvent_ability_conduit_arc_flash( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity weaponOwner = weapon.GetWeaponOwner()














	if ( weaponOwner != GetLocalClientPlayer() )
		return


	weapon.PlayWeaponEffect( FX_TAC_MUZZLE_FLASH, FX_TAC_MUZZLE_FLASH, MUZZLE_ATTACH )

	entity bestTarget = weaponOwner.GetPlayerNetEnt( CONDUIT_ARC_FLASH_BEST_TARGET_NETVAR )
	bool hasValidTarget = IsValid(bestTarget )





	return weapon.GetAmmoPerShot()
}

array<entity> function Conduit_GetArrayOfPossibleAllies( entity conduit, bool includeAlliance = true )
{
	array<entity> allyArray
	int conduitTeam = conduit.GetTeam()

	if ( includeAlliance && AllianceProximity_IsUsingAlliances() )
	{
		int conduitAlliance = AllianceProximity_GetAllianceFromTeam( conduitTeam )
		allyArray = AllianceProximity_GetAllPlayersInAlliance( conduitAlliance, true )
	}
	else
	{
		allyArray = GetPlayerArrayOfTeam_Alive( conduitTeam )
	}
	allyArray.removebyvalue( conduit )

	return allyArray
}












































int function IsValidTacTarget( entity user, entity target, bool ignoreFaceWhenClose )
{
	if ( !IsValid( user ) )
		return eLockResult.FAILED_GENERIC

	if ( !IsValid( target ) )
		return eLockResult.FAILED_GENERIC

	if ( !IsFriendlyTeam( user.GetTeam(), target.GetTeam() ) )
		return eLockResult.FAILED_GENERIC

	if ( Bleedout_IsBleedingOut( target ) )
		return eLockResult.FAILED_GENERIC

	if ( target.IsPhaseShifted() )
		return eLockResult.FAILED_GENERIC

	if ( GetRespawnStatus( target ) != eRespawnStatus.NONE )
		return eLockResult.FAILED_GENERIC

	float distanceSqr = DistanceSqr( user.GetOrigin(), target.GetOrigin() )
	if ( ignoreFaceWhenClose && distanceSqr < ARC_FLASH_MIN_RANGE_SQR )
		return eLockResult.SUCCESS

	entity tacWeapon = user.GetOffhandWeapon( OFFHAND_TACTICAL )
	float EFFECTIVE_RANGE_SQR = GetArcFlashRangeSqr( user )
	if ( distanceSqr > EFFECTIVE_RANGE_SQR )
		return eLockResult.FAILED_OUT_OF_RANGE

	
	float minDot = TARGETING_CONE_DOT
	float dot = DotProduct( Normalize( target.GetWorldSpaceCenter() - user.CameraPosition() ), user.GetViewVector() )
	if ( dot < minDot )
		return eLockResult.FAILED_NOT_FACING_ALLY


	return eLockResult.SUCCESS
}

float function ScoreTarget( entity player, entity target )
{
	const float WEIGHT_SHIELD_FRAC = 25
	const float WEIGHT_DIST = 0
	const float WEIGHT_ANGLE = 35

	float score = 0.0
	if ( !IsValid( target ) )
		return score

	
	bool isValidTacTarget = IsValidTacTarget( player, target, true ) == eLockResult.SUCCESS
	if ( !isValidTacTarget )
		return score

	string scoreDebugString = ""

	float shieldFrac = 0
	if( target.GetShieldHealthMax() > 0 )
		shieldFrac = float(target.GetShieldHealth()) / float(target.GetShieldHealthMax())
	float shieldScore = GraphCapped( shieldFrac, 0.0, 1.0, WEIGHT_SHIELD_FRAC, 0 )
	score += shieldScore
	scoreDebugString += "-shieldScore: " + shieldScore + "\n"

	
	float distanceSqr = DistanceSqr( target.GetOrigin(), player.GetOrigin() )
	float distanceScore = GraphCapped( distanceSqr, ARC_FLASH_MIN_RANGE_SQR, GetArcFlashRangeSqr( player ), WEIGHT_DIST, 0 )
	score += distanceScore
	scoreDebugString += "-distanceScore: " + distanceScore + "\n"

	
	float dot = DotProduct( Normalize( target.GetWorldSpaceCenter() - player.CameraPosition() ), player.GetViewVector() )
	float coneScore = GraphCapped( dot, TARGETING_CONE_DOT, DOT_5DEGREE, 0, WEIGHT_ANGLE )
	score += coneScore
	scoreDebugString += "-coneScore: " + coneScore + "\n"

	int overshieldAmt = target.GetTempshieldHealth()
	scoreDebugString = "Total: " + score + "\n" + scoreDebugString + "\nOvershield: " + overshieldAmt

	if( ARC_FLASH_DEBUG )
	{
		DebugDrawText( target.GetWorldSpaceCenter(), scoreDebugString ,false, 0.1 )
	}

	return score
}

















































































































































































































































































































































































int function GetArcFlashState( entity player )
{
	float severity = StatusEffect_GetSeverity( player, eStatusEffect.shields_repairing )

	
	int state = eArcFlashState.NONE
	if ( severity >= ARC_FLASH_REGEN_SEVERITY )
		state = eArcFlashState.CHARGE
	else if ( severity > ARC_FLASH_DECAY_SEVERITY )
		state = eArcFlashState.ACTIVE
	else if ( severity > 0 )
		state = eArcFlashState.DECAY

	return state
}















asset function GetRealShieldIcon( int shieldState )
{
	asset shieldIcon = $""

	switch ( shieldState )
	{
		case eShieldState.FULL:
			shieldIcon = $"rui/hud/character_abilities/conduit_tactical_spotting_1"
			break
		case eShieldState.HIGH:
			shieldIcon = $"rui/hud/character_abilities/conduit_tactical_spotting_4"
			break
		case eShieldState.LOW:
			shieldIcon = $"rui/hud/character_abilities/conduit_tactical_spotting_2"
			break
		case eShieldState.CRITICAL:
			shieldIcon = $"rui/hud/character_abilities/conduit_tactical_spotting_3"
			break
	}

	return shieldIcon
}

void function TargetingHUD_Thread( entity player )
{
	EndSignal( player, "OnDeath" )
	EndSignal( player, "OnDestroy" )
	EndSignal( player, "TargetingStop" )

	while ( true )
	{
		array<entity> allyArray = Conduit_GetArrayOfPossibleAllies( player )

		foreach ( ally in allyArray )
		{
			if ( !file.trackedAllys[player].contains(ally) )
			{
				thread SingleTargetRui_Thread( player, ally )
			}
		}
		WaitFrame()
	}
}

void function SingleTargetRui_Thread(  entity player, entity target )
{
	if ( !IsValid( target ) )
		return

	EndSignal( target, "OnDestroy", "OnDeath" )
	EndSignal( target, "OnModelChanged" )
	EndSignal( player, "TargetingStop" )
	EndSignal( player, "OnDestroy" )

	file.trackedAllys[player].append(target)

	var rui = RuiCreate( $"ui/conduit_simplified_ally_ui.rpak", clGlobal.topoFullScreen, RUI_DRAW_HUD, RuiCalculateDistanceSortKey( player.EyePosition(), target.GetOrigin() ) )
	InitHUDRui( rui )

	RuiSetBool( rui, "isVisible", false )

	RuiKeepSortKeyUpdated( rui, true, "pos" )

	RuiTrackFloat3( rui, "pos", target, RUI_TRACK_POINT_FOLLOW, target.LookupAttachment( "CHESTFOCUS" )  )

	entity tacticalWeapon       = player.GetOffhandWeapon( OFFHAND_TACTICAL )
	RuiTrackFloat( rui, "tacAmmoFrac", tacticalWeapon, RUI_TRACK_WEAPON_CLIP_AMMO_FRACTION )

	RuiTrackFloat( rui, "healTimeRemaining", target, RUI_TRACK_STATUS_EFFECT_TIME_REMAINING, eStatusEffect.shields_repairing )

	int teamMemberIndex = int( max( target.GetTeamMemberIndex(), 0 ) )
	vector teamMemberColor = GetKeyColor( COLORID_MEMBER_COLOR0, teamMemberIndex )
	RuiSetFloat3( rui, "teamMemberColor", SrgbToLinear( teamMemberColor / 255.0 ) )

	target.DoModelChangeScriptCallback( true )
	OnThreadEnd(
		function() : ( rui, target, player)
		{
			RuiDestroyIfAlive( rui )
			if ( IsValid(player) )
				file.trackedAllys[player].removebyvalue(target)

			if ( IsValid( target ) )
			{
				target.DoModelChangeScriptCallback( false )
				target.SetTargetInfoStatusIcon( $"" )
			}
		}
	)

	int shieldState = eShieldState.INVALID
	float shieldFracLast = -1.0
	while ( true )
	{
		
		entity passiveTarget = player.GetPlayerNetEnt( CONDUIT_PASSIVE_BEST_TARGET_NETVAR )
		bool isPassiveTarget = IsValid( passiveTarget ) && target == passiveTarget

		int state = GetArcFlashState( target )
		int tacTargetResult = IsValidTacTarget( player, target, false )
		bool isValidTacTarget = tacTargetResult == eLockResult.SUCCESS
		bool isOutOfRange = tacTargetResult == eLockResult.FAILED_OUT_OF_RANGE

		bool isVisible = ( isValidTacTarget || isPassiveTarget ) && IsPlayerInValidTacState( player )
		RuiSetBool( rui, "isVisible", isVisible )

		
		
		bool enemyObstructing = false
		const float CONDUIT_TRACE_EXTENTS = 6
		const vector CONDUIT_TRACE_BOUND_MINS = <-CONDUIT_TRACE_EXTENTS, -CONDUIT_TRACE_EXTENTS, -CONDUIT_TRACE_EXTENTS>
		const vector CONDUIT_TRACE_BOUND_MAXS = <CONDUIT_TRACE_EXTENTS, CONDUIT_TRACE_EXTENTS, CONDUIT_TRACE_EXTENTS>
		array<entity> ignoreEnts = [ player, target ]
		TraceResults enemyTrace = TraceHull( player.EyePosition(), target.GetWorldSpaceCenter(), CONDUIT_TRACE_BOUND_MINS, CONDUIT_TRACE_BOUND_MAXS, ignoreEnts, TRACE_MASK_PLAYERSOLID, TRACE_COLLISION_GROUP_NONE, UP_VECTOR, player )
		if ( enemyTrace.fraction < 1.0 )
		{
			if ( enemyTrace.hitEnt.IsPlayer() && !IsFriendlyTeam( enemyTrace.hitEnt.GetTeam(), player.GetTeam() ) )
			{
				
				
				enemyObstructing = true
			}
		}
		RuiSetBool( rui, "enemyObstructing", enemyObstructing )
		


		
		entity bestTarget = player.GetPlayerNetEnt( CONDUIT_ARC_FLASH_BEST_TARGET_NETVAR )
		bool isBestTarget = IsValid(bestTarget) && target == bestTarget
		RuiSetBool( rui, "isBestTarget", isBestTarget )

		
		RuiSetBool( rui, "showPassive", isPassiveTarget )
		if ( isPassiveTarget )
		{
			const float MIN_TIME_TO_TRIGGER = 2.0
			float chargeEndTime = player.GetPlayerNetTime( CONDUIT_PASSIVE_CHARGE_END_NETVAR )
			float chargeTimeRemaining = chargeEndTime != -1 ? max( chargeEndTime - Time(), 0.0 ) : MIN_TIME_TO_TRIGGER
			RuiSetFloat( rui, "passiveFill", 1.0 - chargeTimeRemaining / MIN_TIME_TO_TRIGGER )
		}

		
		float shieldFrac = GetShieldHealthFrac( target )
		if ( shieldFracLast != shieldFrac )
		{
			shieldFracLast = shieldFrac

			const float SHIELD_FRAC_CRITICAL = 0.2
			const float SHIELD_FRAC_SAFE = 0.6

			if ( shieldFrac <= SHIELD_FRAC_CRITICAL )
				shieldState = eShieldState.CRITICAL
			else if ( shieldFrac <= SHIELD_FRAC_SAFE )
				shieldState = eShieldState.LOW
			else if ( shieldFrac < 1.0 )
				shieldState = eShieldState.HIGH
			else
				shieldState = eShieldState.FULL

			RuiSetAsset( rui, "shieldIcon", GetRealShieldIcon( shieldState ) )
		}

		
		if ( target.GetPlayerNetBool( TEMPSHIELD_ACTIVE_NETVAR ) )
		{
			
			if ( state != eArcFlashState.NONE )
			{
				RuiSetInt( rui, "activeState", state )
				RuiSetFloat( rui, "healTimeDuration", GetArcFlashDuration( player, state ) )
			}

			SetUnitFrameOvershieldChargingState( target, state == eArcFlashState.CHARGE )

			if ( !isOutOfRange )
				target.SetTargetInfoStatusIcon( $"rui/hud/character_abilities/conduit_tactical_enemy_shielded" )
			else
				target.SetTargetInfoStatusIcon( $"" )
		}
		else
		{
			RuiSetInt( rui, "activeState", eArcFlashState.NONE )

			SetUnitFrameOvershieldChargingState( target, false )

			if ( !isOutOfRange )
				target.SetTargetInfoStatusIcon( GetRealShieldIcon( shieldState ) )
			else
				target.SetTargetInfoStatusIcon( $"" )
		}


		WaitFrame()
	}
}

bool function IsPlayerInValidTacState( entity player )
{
	if ( Bleedout_IsBleedingOut(player) )
		return false

	if ( player.Player_IsSkydiving() )
		return false

	if ( player.IsDrivingVehicle() )
		return false

	return true
}




void function ArcFlash_StartShieldsRepairing( entity ent, int statusEffect, bool actuallyChanged )
{
	if ( !actuallyChanged && GetLocalViewPlayer() == GetLocalClientPlayer() )
		return

	if ( ent != GetLocalViewPlayer() )
		return

	entity viewPlayer = GetLocalViewPlayer()

	thread ArcFlash_ShieldsRepairingThread( viewPlayer )
}

void function ArcFlash_StopShieldsRepairing( entity ent, int statusEffect, bool actuallyChanged )
{
	if ( !actuallyChanged && GetLocalViewPlayer() == GetLocalClientPlayer() )
		return

	if ( ent != GetLocalViewPlayer() )
		return

	ent.Signal( "ArcFlash_EndShieldsRepairing" )
}

var function GetConduitShieldRui()
{
	return file.shieldsRepairingRui
}

var function ArcFlash_DestroyFXAfterDelay_Thread( entity player, int fxHandle, float delay )
{
	Assert( IsNewThread(), "Must be threaded" )

	player.EndSignal( "ArcFlash_EndShieldsRepairing" )
	player.EndSignal( "OnDestroy" )
	player.EndSignal( "OnDeath" )

	wait delay

	if ( EffectDoesExist( fxHandle ) )
		EffectStop( fxHandle, false, true )
}

void function ArcFlash_ShieldsRepairingThread( entity player )
{
	player.EndSignal( "ArcFlash_EndShieldsRepairing" )
	player.EndSignal( "OnDestroy" )
	player.EndSignal( "OnDeath" )

	int state = GetArcFlashState( player )
	if ( state == eArcFlashState.NONE )
		return

	
	if ( file.shieldsRepairingRui == null )
	{
		file.shieldsRepairingRui = CreateCockpitPostFXRui( $"ui/shields_repairing_indicator.rpak", HUD_Z_BASE )
	}

	RuiTrackFloat( file.shieldsRepairingRui, "timeRemaining", player, RUI_TRACK_STATUS_EFFECT_TIME_REMAINING, eStatusEffect.shields_repairing )

	
	int fxHandle = -1
	if ( state == eArcFlashState.CHARGE )
	{
		int fxID = GetParticleSystemIndex( FX_TEMPSHIELD_1P )
		entity cockpit = player.GetCockpit()
		if ( !IsValid(cockpit) )
			return
		fxHandle = StartParticleEffectOnEntity( cockpit, fxID, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
		EffectSetIsWithCockpit( fxHandle, true )
		EffectSetControlPointVector( fxHandle, 1, TEMPSHIELD_COLOR )
	}

	OnThreadEnd(
		function() : ( player, fxHandle, state )
		{
			if ( IsValid(player) )
			{
				SetCustomPlayerInfoOvershieldChargingState( player, false )
			}

			RuiDestroyIfAlive( file.shieldsRepairingRui )
			file.shieldsRepairingRui = null

			if ( EffectDoesExist( fxHandle ) )
				EffectStop( fxHandle, false, true )

			if ( state == eArcFlashState.DECAY && !player.GetPlayerNetBool( TEMPSHIELD_ACTIVE_NETVAR ) )
				AddPlayerHint( 4.0, 0.5, $"", "#HINT_CONDUIT_SHIELDS_DEPLETED" )
		}
	)

	
	thread ArcFlash_DestroyFXAfterDelay_Thread( player, fxHandle, SOUND_TEMPSHIELD_CHARGE_FINISHING_DURATION )

	
	while ( true )
	{
		RuiSetInt( file.shieldsRepairingRui, "state", state )
		RuiSetFloat( file.shieldsRepairingRui, "timeTotal", GetArcFlashDuration( player, state ) )

		SetCustomPlayerInfoOvershieldChargingState( player, state == eArcFlashState.CHARGE )

		WaitFrame()
		state = GetArcFlashState( player )
	}
}



#if DEV
void function DebugScreenInfo( entity player, array<entity> allyList, entity bestTarget )
{
	
	
	vector color = <0, 100, 200>
	string text  = "Conduit:"

	
	text += "\n# Allys: " + allyList.len()

	text += "\nBestTarget:" + bestTarget

	DebugDrawScreenTextWithColor( 0.7, 0.8, text, color )
}

void function DebugDrawLockOns( entity player, array<entity> allyList, entity bestTarget )
{
	if ( !IsValid(player) )
		return


	foreach( target in allyList )
	{
		if( IsValid(target) )
		{
			DebugDrawSphere( target.GetWorldSpaceCenter(), 15, COLOR_CYAN, true, 0.1 )
			float thisLockScore = ScoreTarget( player, target )
			if ( target == bestTarget )
			{
				DebugDrawText( target.EyePosition(), "BEST", false, 0.1 )
			}
		}
	}
}
#endif
