global function MpWeaponRiotDrill_Init

global function OnWeaponActivate_riot_drill
global function OnWeaponDeactivate_riot_drill
global function OnWeaponTossReleaseAnimEvent_weapon_riot_drill
global function OnProjectileCollision_weapon_riot_drill

global function CodeCallback_BreachTraceEarlyExitOnEnt
global function CodeCallback_BreachTraceIsValidPos


global function OnClientAnimEvent_weapon_riot_drill


global const string RIOT_DRILL_SCRIPT_NAME = "riot_drill_spike"
global const string RIOT_DRILL_DANGERZONE_TARGETNAME = "riot_drill_dangerzone_threat"
const string RIOT_DRILL_MOVER_SCRIPTNAME = "riot_drill_mover"


const asset RIOT_DRILL_SPIKE 					= $"mdl/props/madmaggie_tactical_drill_bit/madmaggie_tactical_drill_bit.rmdl"
const asset RIOT_DRILL_DRILL		 			= $"mdl/props/madmaggie_tactical_drill_bit/madmaggie_tactical_drill_bit.rmdl"


const asset RIOT_DRILL_EMPTY_MODEL				= $"mdl/dev/empty_model.rmdl"
const asset RIOT_DRILL_BLAST_BEAM_FX 			= $"P_mm_breach_beam"
const asset RIOT_DRILL_BLAST_BEAM_WARN_FX 		= $"P_mm_breach_beam_warn"
const asset RIOT_DRILL_AOE_WARNING_01_FX 		= $"P_mm_breach_exit"

    const asset RIOT_DRILL_BLAST_BEAM_WARN_FX_UPGRADE 		= $"P_mm_breach_beam_warn_big"
    const asset RIOT_DRILL_AOE_WARNING_01_FX_UPGRADE 		= $"P_mm_breach_exit_big"

const asset RIOT_DRILL_FRONT_FX 				= $"P_mm_breach_enter"
const asset RIOT_DRILL_DECAL					= $"P_mm_breach_decal"
const asset RIOT_DRILL_ENTER_FX_DEFAULT			= $"P_mm_breach_imp_enter_default"

const vector RIOT_DRILL_PLACEMENT_VALID_COLOR 	= <128, 188, 255>
const vector RIOT_DRILL_PLACEMENT_CAUTION_COLOR = <255, 200, 40>
const vector RIOT_DRILL_PLACEMENT_ERROR_COLOR 	= <255, 40, 40>


const string RIOT_DRILL_DAMAGE_SOUND_1P 		= "flesh_thermiteburn_3p_vs_1p"					
const string RIOT_DRILL_DAMAGE_SOUND_3P 		= "flesh_thermiteburn_3p_vs_3p"				
const string RIOT_DRILL_EXIT_DRILLING 			= "Maggie_Tac_Drill_Exit_Drilling"
const string RIOT_DRILL_ENTRANCE_DRILLING 		= "Maggie_Tac_Drill_Entrance_Drilling"  

const string RIOT_DRILL_EXIT_DRILLING_UPGRADE	= "Maggie_Tac_Drill_Exit_Drilling_Short"


const bool RIOT_DRILL_DEBUG = false

enum eBreachPlacementResult
{
	SUCCESS = BREACH_TRACE_RESULT_SUCCESS,
	FAILED_WALL_TOO_THIN = BREACH_TRACE_RESULT_WALL_TOO_THIN,
	FAILED_WALL_TOO_THICK = BREACH_TRACE_RESULT_WALL_TOO_THICK,
	FAILED_OUT_OF_RANGE = BREACH_TRACE_RESULT_COUNT + 1,
	FAILED_SAFETY_CATCH = BREACH_TRACE_RESULT_INVALID_END_POINT,
	FAILED_GENERIC = BREACH_TRACE_RESULT_FAILURE,
}

global struct RiotDrillPlacementInfo
{
	vector startOrigin
	vector startAngles
	vector startSurfaceNormal
	vector endSurfaceNormal
	int    placementResult
	bool   hide
	entity hitEnt

	vector endOrigin
	vector endAngles
}

struct RiotDrillSystem
{
	entity riotDrillStart
	entity riotDrillEnd
	entity riotDrillDrillMover
	entity riotDrillDrillModel
	entity riotDrillStuckEntity
	entity damageTrigger
	entity riotDrillSoundDummy

	vector dangerZoneOrigin
	vector dangerZoneAngle
	vector breachAngle

	RiotDrillPlacementInfo& placementInfo
}

struct
{
	array<string> shieldScriptNames
	array<string> bounceOffSpecialCaseNames


		bool breachChargeDeployed = false
		var depthRui


	table riotDrillDamageParams 		= { damageSourceId = eDamageSourceId.mp_weapon_concussive_breach, damageType = DMG_BURN }
	table riotDrillImpactDamageParams 	= { damageSourceId = eDamageSourceId.mp_weapon_concussive_breach }

	
	bool fxOption_hideModels
	float fxOption_impactTableFXEnterRefire
	float fxOption_impactTableFXExitRefire
}
file

struct
{
	float damageTickRate = 0.2		
	float delay = 1.0				
	float duration = 8.0			
	int   damage = 4				
	int   impactDamage = 5			
	int   objectDamage = 1			
	float maxThickness = 512.0		
	float radius = 130.0			
	float length = 224.0			
	float range = 1750.0 			
	bool  persistAfterDeath = true	

	float extraChargeDurationOverride = 6.0
	float lengthUpgradeMultiplier = 1.5
	float radiusUpgradeMultiplier = 1.5
	float maxThicknessUpgradeOverride = 768.0

} tuning

void function MpWeaponRiotDrill_Init()
{
	PrecacheParticleSystem( RIOT_DRILL_FRONT_FX )
	PrecacheParticleSystem( RIOT_DRILL_AOE_WARNING_01_FX )

	    PrecacheParticleSystem( RIOT_DRILL_BLAST_BEAM_WARN_FX_UPGRADE )
	    PrecacheParticleSystem( RIOT_DRILL_AOE_WARNING_01_FX_UPGRADE )

	PrecacheParticleSystem( RIOT_DRILL_BLAST_BEAM_FX )
	PrecacheParticleSystem( RIOT_DRILL_BLAST_BEAM_WARN_FX )
	PrecacheParticleSystem( RIOT_DRILL_DECAL )
	PrecacheParticleSystem( RIOT_DRILL_ENTER_FX_DEFAULT )

	PrecacheScriptString( RIOT_DRILL_SCRIPT_NAME )

	PrecacheModel( RIOT_DRILL_SPIKE )
	PrecacheModel( RIOT_DRILL_DRILL )
	PrecacheModel( RIOT_DRILL_EMPTY_MODEL )

	RegisterSignal( "DeployableBreachChargePlacement_End" )
	RegisterSignal( "RiotDrill_TempAnimWindDown" )
	RegisterSignal( "RiotDrill_StuckEntDissolving" )

	SetupTuning()

	
	file.fxOption_impactTableFXEnterRefire	= GetCurrentPlaylistVarFloat( "breaching_spike_impact_fx_enter_refire", 0.2 )
	file.fxOption_impactTableFXExitRefire	= GetCurrentPlaylistVarFloat( "breaching_spike_impact_fx_exit_refire", 0.2 )

	file.shieldScriptNames.append( MOBILE_SHIELD_SCRIPTNAME )
	file.shieldScriptNames.append( BUBBLE_SHIELD_SCRIPTNAME )
	file.shieldScriptNames.append( AMPED_WALL_SCRIPT_NAME )
	file.shieldScriptNames.append( ECHO_LOCATOR_SCRIPT_NAME )

	file.bounceOffSpecialCaseNames.append( "pathfinder_tt_ring_shield" )
	file.bounceOffSpecialCaseNames.append( DEATHBOX_FLYER_SCRIPT_NAME )












		AddTargetNameCreateCallback( RIOT_DRILL_DANGERZONE_TARGETNAME, RiotDrill_AddThreatIndicator )

		AddCallback_OnViewPlayerChanged( OnViewPlayerChanged )

}

void function SetupTuning()
{
	tuning.damageTickRate    = GetCurrentPlaylistVarFloat( "riot_drill_damageTickRate", tuning.damageTickRate )
	tuning.delay             = GetCurrentPlaylistVarFloat( "breaching_spike_delay_override", tuning.delay )
	tuning.duration          = max( GetCurrentPlaylistVarFloat( "riot_drill_duration_override", tuning.duration ), 0.0 )
	tuning.damage            = maxint( GetCurrentPlaylistVarInt( "breaching_spike_damage_override", tuning.damage ), 0 )
	tuning.impactDamage      = maxint( GetCurrentPlaylistVarInt( "breaching_spike_impact_damage_override", tuning.impactDamage ), 0 )
	tuning.objectDamage      = maxint( GetCurrentPlaylistVarInt( "breaching_spike_stuck_damage_override", tuning.objectDamage ), 0 )
	tuning.maxThickness      = GetCurrentPlaylistVarFloat( "breaching_spike_max_thickness_override", tuning.maxThickness )
	tuning.radius            = GetCurrentPlaylistVarFloat( "breaching_spike_radius_override", tuning.radius )
	tuning.length            = GetCurrentPlaylistVarFloat( "breaching_spike_length_override", tuning.length )
	tuning.range             = GetCurrentPlaylistVarFloat( "breaching_spike_range_override", tuning.range )
	tuning.persistAfterDeath = GetCurrentPlaylistVarBool( "breaching_spike_after_death_override", tuning.persistAfterDeath )
	tuning.extraChargeDurationOverride = GetCurrentPlaylistVarFloat( "riot_drill_extraChargeDurationOverride", tuning.extraChargeDurationOverride )
	tuning.lengthUpgradeMultiplier = GetCurrentPlaylistVarFloat( "riot_drill_lengthUpgradeMultiplier", tuning.lengthUpgradeMultiplier )
	tuning.radiusUpgradeMultiplier = GetCurrentPlaylistVarFloat( "riot_drill_radiusUpgradeMultiplier", tuning.radiusUpgradeMultiplier )
	tuning.maxThicknessUpgradeOverride      = GetCurrentPlaylistVarFloat( "riot_drill_maxThicknessUpgradeOverride", tuning.maxThickness )
}

#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________Accessors___________________________(){}
#endif
entity function GetRiotDrillFromPlayerIfActive( entity player )
{
	if ( IsAlive( player ) )
	{
		entity weapon = player.GetOffhandWeapon( OFFHAND_SPECIAL )
		if ( IsValid( weapon ) && weapon.GetWeaponClassName() == "mp_weapon_riot_drill" )
			return weapon
	}

	return null
}

void function RestoreRiotDrillAmmo( entity owner )
{
	entity weapon = GetRiotDrillFromPlayerIfActive( owner )
	if ( weapon != null )
	{
		Weapon_AddSingleCharge( weapon )
	}
}

float function RiotDrill_GetDuration( entity player )
{
	float result = tuning.duration

		if( PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_ONE ) ) 
			result = tuning.extraChargeDurationOverride

	return result
}

float function RiotDrill_GetRadius( entity player )
{
	float result = tuning.radius

	if( PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_TWO ) ) 
		result *= tuning.radiusUpgradeMultiplier

	return result
}

float function RiotDrill_GetLength( entity player )
{
	float result = tuning.length

		if( PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_TWO ) ) 
			result *= tuning.lengthUpgradeMultiplier

	return result
}











































































































































































































































































































































































































































































































































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________ClientFuncs___________________________(){}
#endif


void function OnClientAnimEvent_weapon_riot_drill( entity weapon, string name )
{
	if ( !IsValid( weapon ) )
		return

	const float SHAKE_AMPLITUDE = 2.0
	const float SHAKE_FREQUENCY = 10.0
	const float SHAKE_DURATION = 0.2
	const vector SHAKE_DIRECTION = < 0.0, 0.0, 1.0 >

	if ( name == "riot_drill_screen_shake" )
		ClientScreenShake( SHAKE_AMPLITUDE, SHAKE_FREQUENCY, SHAKE_DURATION, SHAKE_DIRECTION )
}

void function SetBreachChargeDeployed( bool state )
{
	file.breachChargeDeployed = state
}

void function DeployableBreachChargePlacementThink( entity player, entity weapon )
{
	EndSignal( player, "DeployableBreachChargePlacement_End", "OnDeath", "OnDestroy", SIGNAL_BLEEDOUT_STATE_CHANGED )
	EndSignal( weapon, "OnDestroy" )

	const vector COLOR_DEPTH_UNKNOWN	= <255, 122, 0>
	const vector COLOR_DEPTH_START 		= <255, 122, 0>
	const vector COLOR_DEPTH_MID 		= <255, 210, 73>
	const vector COLOR_DEPTH_END 		= <255, 255, 255>

	file.depthRui = CreateFullscreenRui( $"ui/mm_riot_drill.rpak" )

	OnThreadEnd(
		function() : ( weapon )
		{
			if ( file.depthRui != null )
			{
				RuiDestroyIfAlive( file.depthRui )
				file.depthRui = null
			}
			weapon.SetGrenadeIndicatorEffectUseBlockedFX( false )
		}
	)

	bool previousResultWasSuccess = true
	while ( player.IsUsingOffhandWeapon( eActiveInventorySlot.altHand ) )
	{
		RiotDrillPlacementInfo placementInfo = GetRiotDrillPlacementInfo( player, weapon, [] )

		float distance = -1.0
		vector color = COLOR_DEPTH_UNKNOWN

		bool resultWasSuccess = true
		switch ( placementInfo.placementResult )
		{
			case eBreachPlacementResult.FAILED_GENERIC:
			case eBreachPlacementResult.FAILED_OUT_OF_RANGE:
			case eBreachPlacementResult.FAILED_WALL_TOO_THICK:
			case eBreachPlacementResult.FAILED_SAFETY_CATCH:
				resultWasSuccess = false
				break
			default:
				distance = Distance( placementInfo.startOrigin, placementInfo.endOrigin )
				float distanceFrac = distance / tuning.maxThickness
				color = GetTriLerpColor( distanceFrac, COLOR_DEPTH_END, COLOR_DEPTH_MID, COLOR_DEPTH_START, 0.6, 0.3 )
				resultWasSuccess = true
		}

		if ( resultWasSuccess != previousResultWasSuccess )
		{
			previousResultWasSuccess = resultWasSuccess
			weapon.SetGrenadeIndicatorEffectUseBlockedFX( !resultWasSuccess )
		}

		RuiSetFloat( file.depthRui, "depth", distance )
		RuiSetFloat3( file.depthRui, "infoTextColorRGB", ( color / 255.0 ) )

		WaitFrame()
	}
}

void function RiotDrill_AddThreatIndicator( entity dangerZone )
{
	entity player = GetLocalViewPlayer()

	entity owner = dangerZone.GetOwner()
	int team = player.GetTeam()
	int dangerZoneTeam = player.GetTeam()

	if( IsEnemyTeam( team, dangerZoneTeam ) || ( player == owner ) )
		ShowGrenadeArrow( player, dangerZone, RiotDrill_GetLength( owner ), 0, false, eThreatIndicatorVisibility.INDICATOR_SHOW_TO_ALL, ( 60.0 * dangerZone.GetUpVector() ) )
}



#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________WeaponFuncs___________________________(){}
#endif


void function OnViewPlayerChanged( entity player )
{
	player.Signal( "DeployableBreachChargePlacement_End" )

	entity weapon = GetRiotDrillFromPlayerIfActive( player )
	if ( weapon != null )
	{
		thread DeployableBreachChargePlacementThink( player, weapon )
	}
}


var function OnWeaponTossReleaseAnimEvent_weapon_riot_drill( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity player = weapon.GetOwner()

	bool ignite = false





		SetBreachChargeDeployed( true )
		player.Signal( "DeployableBreachChargePlacement_End" )


	var result = RiotDrill_FireProjectile( weapon, attackParams, ignite )
	return result
}


int function RiotDrill_FireProjectile( entity weapon, WeaponPrimaryAttackParams attackParams, bool ignite )
{
	weapon.EmitWeaponSound_1p3p( GetGrenadeThrowSound_1p( weapon ), GetGrenadeThrowSound_3p( weapon ) )
	bool projectilePredicted      = PROJECTILE_PREDICTED
	bool projectileLagCompensated = PROJECTILE_LAG_COMPENSATED







	entity grenade     = Grenade_Launch( weapon, attackParams.pos, (attackParams.dir), projectilePredicted, projectileLagCompensated, ZERO_VECTOR )
	entity weaponOwner = weapon.GetWeaponOwner()
	weaponOwner.Signal( "ThrowGrenade" )

	PlayerUsedOffhand( weaponOwner, weapon, true, grenade )

	if ( IsValid( grenade ) )
	{
		grenade.proj.savedDir = weaponOwner.GetViewForward()
		grenade.proj.savedOrigin = grenade.GetOrigin()
	}












	return weapon.GetAmmoPerShot()
}

void function OnWeaponActivate_riot_drill( entity weapon )
{
	entity ownerPlayer = weapon.GetWeaponOwner()
	Assert( ownerPlayer.IsPlayer() )


		SetBreachChargeDeployed( false )
		if ( !InPrediction() ) 
			return
		if ( ownerPlayer == GetLocalViewPlayer() )
			thread DeployableBreachChargePlacementThink( ownerPlayer, weapon )

}
void function OnWeaponDeactivate_riot_drill( entity weapon )
{
	entity ownerPlayer = weapon.GetWeaponOwner()
	Assert( ownerPlayer.IsPlayer() )


		if ( !InPrediction() ) 
			return
		if ( ownerPlayer == GetLocalViewPlayer() )
			ownerPlayer.Signal( "DeployableBreachChargePlacement_End" )





}

void function OnProjectileCollision_weapon_riot_drill( entity projectile, vector pos, vector normal, entity hitEnt, int hitBox, bool isCritical, bool isPassthrough )
{
	bool isBounceTarget = ( hitEnt.IsPlayer() || hitEnt.IsNPC() || file.bounceOffSpecialCaseNames.contains( hitEnt.GetScriptName() ) )
	
	
	entity hitEntParent = hitEnt.GetParent()
	if ( IsValid( hitEntParent ) )
		isBounceTarget = isBounceTarget || hitEntParent.IsPlayer() || hitEntParent.IsNPC()


















	if ( isBounceTarget )
		return





























































	projectile.SetVelocity( <0, 0, 0> )
	projectile.StopPhysics()

}


#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________Placement___________________________(){}
#endif


RiotDrillPlacementInfo function GetRiotDrillPlacementInfo( entity player, entity weapon, array<entity> ignoreEnts )
{
	int placementResult = eBreachPlacementResult.SUCCESS

	array<entity> ignoreArray = [ player ]
	ignoreArray.extend( GetPlayerDecoyArray() )
	ignoreArray.extend( ignoreEnts )

	vector traceStart = player.EyePosition()
	vector traceEnd	= traceStart + ( player.GetViewVector() * tuning.range )

	TraceResults traceResults = TraceLineHighDetail( traceStart, traceEnd, ignoreArray, TRACE_MASK_SHOT, TRACE_COLLISION_GROUP_NONE, player )
	entity testHitEnt = traceResults.hitEnt
	if ( IsValid( testHitEnt ) )
	{
		if ( IsValid( testHitEnt.GetParent() ) )
			testHitEnt = testHitEnt.GetParent()
	}

	if ( !IsValid( testHitEnt ) )
		placementResult = eBreachPlacementResult.FAILED_OUT_OF_RANGE
	else if ( testHitEnt.IsPlayer() || testHitEnt.IsNPC() || file.bounceOffSpecialCaseNames.contains( testHitEnt.GetScriptName() ) )
		placementResult = eBreachPlacementResult.FAILED_SAFETY_CATCH
	else if ( traceResults.hitEnt.GetPassThroughFlags() != 0 && ( CheckPassThroughDir( traceResults.hitEnt, traceResults.surfaceNormal, traceResults.endPos ) ) )
	{
		if ( ignoreEnts.len() == 0 )
			ignoreEnts = [ traceResults.hitEnt ]
		else
			ignoreEnts.append( traceResults.hitEnt )
		return GetRiotDrillPlacementInfo( player, weapon, ignoreEnts )
	}

	RiotDrillPlacementInfo placementInfo
	placementInfo.startOrigin = traceResults.endPos
	placementInfo.startAngles = player.EyeAngles()
	placementInfo.startSurfaceNormal = traceResults.surfaceNormal
	placementInfo.placementResult = placementResult
	placementInfo.hitEnt = traceResults.hitEnt

	if ( placementInfo.placementResult == eBreachPlacementResult.SUCCESS)
		placementInfo.placementResult = FindEndSpikeLocation( player, placementInfo )

	return placementInfo
}


int function FindEndSpikeLocation( entity player, RiotDrillPlacementInfo placementInfo )
{
	const vector HULL_TRACE_MIN = <-4, -4, 0>
	const vector HULL_TRACE_MAX = <4, 4, 32>

	vector pos                             = placementInfo.startOrigin
	vector forward                         = AnglesToForward( placementInfo.startAngles )

	BreachTraceResults breachTraceTresults

	if( PlayerHasPassive( player, ePassives.PAS_TAC_UPGRADE_TWO ) ) 
	{
		breachTraceTresults = BreachTrace( pos, forward, HULL_TRACE_MIN, HULL_TRACE_MAX, tuning.maxThicknessUpgradeOverride )
	}
	else

	{
		breachTraceTresults = BreachTrace( pos, forward, HULL_TRACE_MIN, HULL_TRACE_MAX, tuning.maxThickness )
	}

	if ( breachTraceTresults.result == BREACH_TRACE_RESULT_SUCCESS )
	{
		placementInfo.endAngles        = placementInfo.startAngles
		placementInfo.endOrigin        = breachTraceTresults.endPos
		placementInfo.endSurfaceNormal = breachTraceTresults.surfaceNormal
	}
	else if ( breachTraceTresults.result == BREACH_TRACE_RESULT_WALL_TOO_THICK )
	{
		placementInfo.endAngles = placementInfo.startAngles
		placementInfo.endOrigin = placementInfo.startOrigin
	}

	return breachTraceTresults.result
}

bool function CodeCallback_BreachTraceEarlyExitOnEnt( entity ent )
{
	if ( IsValid( ent ) && file.shieldScriptNames.contains( ent.GetScriptName() ) )
	{
		return true
	}

	return false
}

bool function CodeCallback_BreachTraceIsValidPos( vector pos )
{










	return true
}