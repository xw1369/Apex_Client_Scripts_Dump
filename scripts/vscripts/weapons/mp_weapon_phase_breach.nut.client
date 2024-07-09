global function MpWeaponPhaseBreach_Init
global function OnWeaponActivate_weapon_phase_breach
global function OnWeaponDeactivate_weapon_phase_breach
global function OnWeaponPrimaryAttack_ability_phase_breach
global function OnWeaponPrimaryAttackAnimEvent_ability_phase_breach
global function OnWeaponAttemptOffhandSwitch_weapon_phase_breach




global function ServerToClient_PhaseBreachPortalCancelled

    global function ServerToClient_NotifyAshCooldownReduction


#if DEV
global function DEV_ClearTargetingData
global function DEV_ToggleAshValidation
#endif


const string SOUND_PORTAL_ENTRANCE_OPEN_1P = "Ash_PhaseBreach_Activate_1p"
const string SOUND_PORTAL_ENTRANCE_OPEN_3P = "Ash_PhaseBreach_Activate_3p"
const string SOUND_PORTAL_EXIT_OPEN = "Ash_PhaseBreach_PortalOpen_Exit_3p"
const string SOUND_PORTAL_LOOP = "Ash_PhaseBreach_Portal_Loop" 
const string SOUND_PORTAL_CLOSE = "Ash_PhaseBreach_Portal_Expire" 

const string SIGNAL_PHASE_BREACH_STOP_PLACEMENT = "PhaseBreach_StopPlacement"

const string FUNCNAME_ENEMY_BREACHED_NEARBY = "PhaseBreach_EnemyBreachedNearby"

const string ABILITY_USED_MOD = "ability_used_mod"

const vector BREACH_OFFSET = <0, 0, 42>
const vector BREACH_HULLCHECK_MINS   = <-5.0, -5.0, 36.0 - 5.0>
const vector BREACH_HULLCHECK_MAXS   = < 5.0,  5.0, 36.0 + 5.0>

const float PHASE_BREACH_SPEED = 1200.0
const float PHASE_BREACH_TRAVEL_TIME_MIN = 0.3


const float PHASE_BREACH_TRAVEL_TIME_MAX_UPGRADED = 1.4

const float PHASE_BREACH_PORTAL_LIFETIME = 15.0




const float PHASE_BREACH_TRAVEL_TIME_MAX = 1.8
const float PHASE_BREACH_MAX_2D_DIST_DEFAULT = 3000.0

const float PHASE_BREACH_MAX_ANGLE_FOR_FULL_DIST_DEFAULT = 45.0
const bool  PHASE_BREACH_ALLOW_START_ON_MOVERS_DEFAULT = true
const bool  PHASE_BREACH_ALLOW_END_ON_MOVERS_DEFAULT = true
const float PHASE_BREACH_MOVERS_MAX_SPEED_FOR_END_DEFAULT = 12.0

const bool  PHASE_BREACH_ALLOW_END_ON_OOB = false



const float PHASE_BREACH_MIN_VIEW_DOT = DOT_15DEGREE

const bool DEBUG_DRAW_TARGETING = false
const bool DEBUG_DRAW_PLACEMENT_TRACES = false
const bool DEBUG_DRAW_ENDING_SCORES    = false
const bool DEBUG_DRAW_PUSHER_MOVEMENT  = false

bool DEV_DO_VALIDATION = true
const bool LOG_VALIDATION_DATA = true
const bool DEBUG_DRAW_VALIDATION = false

const asset BREACH_TARGET_FX = $"P_ar_ping_squad_CP_altZ"
const asset BREACH_STARTPOINT_FX = $"P_ash_breach_start"
const asset BREACH_ENDPOINT_FX = $"P_ash_breach_end"
const asset BREACH_AIM_FX = $"P_wrp_trl_end"

const asset BREACH_FX_AR_DIR = $"P_ar_ping_wall_dir_CP"
const asset BREACH_FX_AR_INVALID = $"P_mm_breach_arc_end_fail"




const string FUNC_BREACH_FAILED = "ServerToClient_PhaseBreachPortalCancelled"
const string PLACEMENT_FAILED_HINT = "#PHASE_BREACH_CANT_PLACE"
global const string PHASE_BREACH_BLOCKER_SCRIPTNAME = "phase_breach_blocker"


struct PhaseBreachTargetInfo
{
	array<vector> posList
	vector        finalPos
	float         pathDistance

	vector        startPos
	bool 		  startCrouched
	bool 		  startBlocked

	vector		eyeTracePos
	vector		eyeTraceNormal

	float         portalQuality
}

struct PhaseBreachTraceResults
{
	TraceResults& results
	vector        adjustedEndPos
	bool          foundValidEnd
}

struct
{
	table<entity, PhaseTunnelPortalData>          triggerStartpoint
	table<entity, PhaseBreachTargetInfo>          portalTargetTable
	table<entity, PhaseTunnelData>                tunnelData





		int		targetingFxHandle
		int    	targetingFxHandleDir
		int    	targetingInvalidFxHandle
		string targetingHint


	float maxDist
	float maxAngleForFullDist
	float maxEndingMoverSpeedSqr
	bool allowStartOnMovers
	bool allowEndOnMovers
	array<string> invalidTriggerEndingTypes = ["trigger_slip"]

	bool breachPersistsWhenAshDies

#if DEV
		int numTargetingRuns
		int newTargetingHits
		int oldTargetingHits

		table<int, string> newTargetingWins
		table<int, string> oldTargetingWins
		table<int, string> oldTargetingBetter

		float newAvgScore
		float oldAvgScore


#endif
} file

void function MpWeaponPhaseBreach_Init()
{
	PrecacheParticleSystem( BREACH_ENDPOINT_FX )
	PrecacheParticleSystem( BREACH_TARGET_FX )
	PrecacheParticleSystem( BREACH_AIM_FX )
	PrecacheParticleSystem( BREACH_STARTPOINT_FX )
	PrecacheParticleSystem( BREACH_FX_AR_DIR )
	PrecacheParticleSystem( BREACH_FX_AR_INVALID )





	PrecacheScriptString( "portal_marker" )

	RegisterSignal( SIGNAL_PHASE_BREACH_STOP_PLACEMENT )

	file.maxDist = GetCurrentPlaylistVarFloat( "ash_phase_breach_max_2d_dist", PHASE_BREACH_MAX_2D_DIST_DEFAULT )
	file.maxAngleForFullDist = GetCurrentPlaylistVarFloat( "ash_phase_breach_max_angle_for_full_dist", PHASE_BREACH_MAX_ANGLE_FOR_FULL_DIST_DEFAULT )
	file.maxEndingMoverSpeedSqr = pow( GetCurrentPlaylistVarFloat( "ash_phase_breach_max_mover_speed", PHASE_BREACH_MOVERS_MAX_SPEED_FOR_END_DEFAULT ), 2.0)
	file.allowStartOnMovers = GetCurrentPlaylistVarBool( "ash_phase_breach_allow_start_on_movers", PHASE_BREACH_ALLOW_START_ON_MOVERS_DEFAULT )
	file.allowEndOnMovers = GetCurrentPlaylistVarBool( "ash_phase_breach_allow_end_on_movers", PHASE_BREACH_ALLOW_END_ON_MOVERS_DEFAULT )




	if ( PHASE_BREACH_ALLOW_END_ON_OOB == false )
	{
		file.invalidTriggerEndingTypes.append( "trigger_out_of_bounds" )
		file.invalidTriggerEndingTypes.append( "trigger_networked_out_of_bounds" )
	}

	Remote_RegisterClientFunction( FUNC_BREACH_FAILED )

		Remote_RegisterClientFunction( "ServerToClient_NotifyAshCooldownReduction" )

	file.breachPersistsWhenAshDies = GetCurrentPlaylistVarBool( "ash_ult_persists_past_ash_death", true )
}


float function GetUpgradedMaxPhaseTravelTime()
{
	return GetCurrentPlaylistVarFloat( "ash_upgraded_max_phase_travel_time", PHASE_BREACH_TRAVEL_TIME_MAX_UPGRADED )
}

float function GetMaxPhaseTravelTime( entity player )
{
	float result = PHASE_BREACH_TRAVEL_TIME_MAX

		if( PlayerHasPassive( player, ePassives.PAS_ULT_UPGRADE_ONE ) ) 
			result = GetUpgradedMaxPhaseTravelTime()

	return result
}


void function OnWeaponActivate_weapon_phase_breach( entity weapon )
{
	bool serverOrPredicted = IsServer() || (InPrediction() && IsFirstTimePredicted())
	if ( serverOrPredicted )
	{
		weapon.RemoveMod( ABILITY_USED_MOD )
	}


		entity player = weapon.GetWeaponOwner()
		if ( player == GetLocalViewPlayer() )
			thread PhaseBreachPlacement_Thread( weapon )





}


void function OnWeaponDeactivate_weapon_phase_breach( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	if ( player in file.portalTargetTable )
		delete file.portalTargetTable[player]










		if ( player == GetLocalViewPlayer() )
			weapon.Signal( SIGNAL_PHASE_BREACH_STOP_PLACEMENT )

}


bool function OnWeaponAttemptOffhandSwitch_weapon_phase_breach( entity weapon )
{
	return true
}


var function OnWeaponPrimaryAttack_ability_phase_breach( entity weapon, WeaponPrimaryAttackParams params )
{
	entity player = weapon.GetWeaponOwner()

	if ( !IsValid( player ) || player.IsPhaseShifted() )
		return 0

	PhaseBreachTargetInfo info = GetPhaseBreachTargetInfo( player )

	if ( info.portalQuality <= 0 )
		return 0

	file.portalTargetTable[player] <- info

	StatusEffect_AddTimed( player, eStatusEffect.move_slow, 0.5, 0.5, 0.0 )






	return weapon.GetAmmoPerShot()
}


var function OnWeaponPrimaryAttackAnimEvent_ability_phase_breach( entity weapon, WeaponPrimaryAttackParams params )
{
	entity player = weapon.GetWeaponOwner()

	if ( !IsValid( player ) || !(player in file.portalTargetTable) )
		return 0

	if ( player.IsPhaseShifted() )
	{
		delete file.portalTargetTable[player]
		return 0
	}

	bool serverOrPredicted = IsServer() || (InPrediction() && IsFirstTimePredicted())
	if ( serverOrPredicted )
	{
		weapon.AddMod( ABILITY_USED_MOD )
	}

	PhaseBreachTargetInfo info = file.portalTargetTable[player]

	PlayerUsedOffhand( player, weapon, false )












































		if ( player == GetLocalViewPlayer() )
			weapon.Signal( SIGNAL_PHASE_BREACH_STOP_PLACEMENT )


	return 0
}



























































































































































































void function ServerToClient_PhaseBreachPortalCancelled()
{
	entity localPlayer = GetLocalViewPlayer()
	if ( !IsValid( localPlayer ) )
		return

	AddPlayerHint( 3.0, 0.5, $"", "Phase Breach Failed" )

	StopSoundOnEntity( localPlayer, "Ash_PhaseBreach_Enter_1p" )
}


void function ServerToClient_NotifyAshCooldownReduction()
{
	entity localViewPlayer = GetLocalViewPlayer()
	if ( IsValid( localViewPlayer ) && PlayerHasPassive( localViewPlayer, ePassives.PAS_ULT_UPGRADE_TWO ) )
	{
		string hintStr = "#PHASE_BREACH_COOLDOWN_REDUCED"
		AddPlayerHint( 2.5, 0.25, $"rui/hud/ultimate_icons/ultimate_ash", hintStr )
	}
}


















































































































































































































const float DOWN_TRACE_DISTANCE = 1000.0
const float BACK_TRACE_STEP_DIST = 50.0
const float BACK_TRACE_MAX_STEP = 200.0
const float TUNNEL_STEP_DIST = 16.0

PhaseBreachTargetInfo function GetPhaseBreachTargetInfo( entity player )
{
	PhaseBreachTargetInfo info
	info.startPos   = player.GetOrigin()
	info.finalPos   = player.GetOrigin()
	info.startCrouched = player.IsCrouched()
	info.eyeTracePos = ZERO_VECTOR
	info.eyeTraceNormal = ZERO_VECTOR

	vector eyePos = player.EyePosition()
	vector eyeDir = player.GetViewVector()
	eyeDir          = Normalize( eyeDir )

	vector mins = player.GetPlayerMins()
	vector maxs = player.GetPlayerMaxs()

	
	if ( !file.allowStartOnMovers )
	{
		entity groundEnt = player.GetGroundEntity()
		if ( GetPusherEnt( groundEnt ) )
			return info
	}

	
	
	if ( ! PhaseTunnel_IsPortalExitPointValid( player, info.startPos, player, true, info.startCrouched ) )
	{
		info.startBlocked = true
		return info
	}

	float rangeNormal = file.maxDist
	float rangeSqr    = rangeNormal * rangeNormal

	
	float pitchClamped   = clamp( player.EyeAngles().x, -file.maxAngleForFullDist, file.maxAngleForFullDist )
	float rangeEffective = rangeNormal / deg_cos( pitchClamped )

	array<entity> ignoredEnts = [ player ]

	
	
	PhaseBreachTraceResults eyeTrace = DoEyeTrace( eyePos, eyeDir, rangeEffective, ignoredEnts, mins, maxs )

#if DEV
	if ( DEBUG_DRAW_TARGETING )
	{
		vector debugColor = eyeTrace.results.fraction < 1.0 ? COLOR_GREEN : COLOR_RED
		DebugDrawSphere(  eyeTrace.results.endPos, 10,debugColor, false,0.1 )

		vector adjustedColor = eyeTrace.foundValidEnd ? COLOR_GREEN : COLOR_ORANGE
		DebugDrawSphere(  eyeTrace.adjustedEndPos,5, adjustedColor, false,0.1 )

		float distMeters = Distance( eyeTrace.results.endPos, player.GetOrigin() ) * INCHES_TO_METERS
		string text = "Ash Ult: " +
						"\nRange " + distMeters + "/" + (file.maxDist*INCHES_TO_METERS) +
						"\nEffective: " + (rangeEffective*INCHES_TO_METERS)
		DebugDrawScreenText( 0.1, 0.6, text )
	}
#endif

	info.eyeTracePos = eyeTrace.results.endPos
	info.eyeTraceNormal = eyeTrace.results.surfaceNormal
	
	if ( eyeTrace.foundValidEnd && IsBreachPositionValid( player, eyeTrace.adjustedEndPos, eyeTrace.results.hitEnt, eyeDir, eyeTrace.results.surfaceNormal ) )
	{
		bool success = GenerateBreachPathInfo( player, info, eyeTrace.adjustedEndPos )

		if ( success )
		{
			info.portalQuality = FLT_MAX
			return info
		}
	}

	if ( eyeTrace.results.fraction >= 1.0 )
	{
		vector lowerEyeDir = VectorRotateAxis( eyeDir, player.GetRightVector(), -1 )
		PhaseBreachTraceResults lowerEyeTrace = DoEyeTrace( eyePos, lowerEyeDir, rangeEffective, ignoredEnts, mins, maxs )
#if DEV
			if ( DEBUG_DRAW_TARGETING )
			{
				DebugDrawText( eyeTrace.results.endPos, "Lower", false, 0.1 )
				vector debugColor = eyeTrace.results.fraction < 1.0 ? COLOR_GREEN : COLOR_RED
				DebugDrawSphere(  eyeTrace.results.endPos, 10,debugColor, false,0.1 )

				vector adjustedColor = eyeTrace.foundValidEnd ? COLOR_GREEN : COLOR_ORANGE
				DebugDrawSphere(  eyeTrace.adjustedEndPos,5, adjustedColor, false,0.1 )
			}
#endif
		if ( lowerEyeTrace.results.fraction < 1.0 )
			eyeTrace = lowerEyeTrace
	}

	info.eyeTracePos = eyeTrace.results.endPos
	info.eyeTraceNormal = eyeTrace.results.surfaceNormal

	
	array<vector> possibleEndings

	if ( eyeTrace.results.fraction < 1.0 )
	{
		PerfStart( PerfIndexClient.PhaseBreach_WallToTop )
		bool surfaceIsWall = !IsNormalVertical( eyeTrace.results.surfaceNormal )

		if ( surfaceIsWall )
		{
			vector flattenedEyeDir = FlattenNormalizeVec( eyeDir )

			const float LEDGE_CHECK_UP = DOWN_TRACE_DISTANCE/2
			const float LEDGE_CHECK_BACK = 24
			float debugDrawTime      = DEBUG_DRAW_TARGETING ? 0.1 : 0.0

			vector wallTraceMaxs = <maxs.x,maxs.y,PHASE_TUNNEL_CROUCH_HEIGHT>
			vector wallToTopNormal = -flattenedEyeDir

			float eyeVsWallDot = DotProduct( eyeTrace.results.surfaceNormal, -flattenedEyeDir )
			if ( eyeVsWallDot < DOT_45DEGREE )
				wallToTopNormal = eyeTrace.results.surfaceNormal

			WallToTopResults results
			float checkUpDistance = LEDGE_CHECK_UP
			while( !results.found && checkUpDistance > 0)
			{
				results = TraceFromWallToTop( eyeTrace.results.endPos, wallToTopNormal, [ player ], LEDGE_CHECK_BACK, checkUpDistance, TRACE_MASK_PLAYERSOLID_BRUSHONLY, TRACE_COLLISION_GROUP_PLAYER, debugDrawTime, true, mins, wallTraceMaxs )
				
				checkUpDistance -= PHASE_TUNNEL_CROUCH_HEIGHT
			}

			
			
			
			
			
			
			

			if ( results.found && IsBreachPositionValid( player, results.pos, results.hitEnt, eyeDir, results.normal ) )
			{
				possibleEndings.append( results.pos )
			}
		}
		PerfEnd( PerfIndexClient.PhaseBreach_WallToTop )
	}

	
	{
		TraceResults downTrace = TraceHull( eyeTrace.adjustedEndPos, eyeTrace.adjustedEndPos - <0.0, 0.0, DOWN_TRACE_DISTANCE>, <-5,-5,-5>, <5,5,5>, ignoredEnts, TRACE_MASK_ABILITY_HULL, TRACE_COLLISION_GROUP_PLAYER )
		DrawDebugSphereIfDebugging( downTrace.endPos, 0, 255, 0 )

		if ( downTrace.fraction < 1.0 )
		{
			TraceResults hullTrace = DoHullTraceForExit( mins, maxs, downTrace.endPos )

			if ( !hullTrace.startSolid && IsBreachPositionValid( player, hullTrace.endPos, hullTrace.hitEnt != null ? hullTrace.hitEnt : downTrace.hitEnt, eyeDir, downTrace.surfaceNormal ) )
			{
				possibleEndings.append( hullTrace.endPos )
			}
		}
	}

	
	{
		vector airPos      = eyeTrace.adjustedEndPos - eyeDir * BACK_TRACE_STEP_DIST
		float airTraceDist = BACK_TRACE_STEP_DIST
		int i              = 0

		while ( possibleEndings.len() < 10 && airTraceDist < (rangeEffective - 250.0) && (DotProduct( eyeTrace.adjustedEndPos - eyePos, airPos - eyePos ) > 0) )
		{
			TraceResults airDownTrace = TraceHull( airPos, airPos - <0.0, 0.0, DOWN_TRACE_DISTANCE>, <-5,-5,-5>, <5,5,5>, ignoredEnts, TRACE_MASK_ABILITY_HULL, TRACE_COLLISION_GROUP_PLAYER )
			DrawDebugSphereIfDebugging( airPos, 0, 255, 255 )
			DrawDebugSphereIfDebugging( airDownTrace.endPos, 255, 0, 255 )
			if ( airDownTrace.fraction < 1.0 )
			{
				TraceResults hullTrace = DoHullTraceForExit( mins, maxs, airDownTrace.endPos )

				if ( !hullTrace.startSolid && IsBreachPositionValid( player, hullTrace.endPos, hullTrace.hitEnt != null ? hullTrace.hitEnt : airDownTrace.hitEnt, eyeDir, airDownTrace.surfaceNormal ) )
				{
					possibleEndings.append( hullTrace.endPos )
				}
				else if ( DEBUG_DRAW_TARGETING )
				{
					if ( hullTrace.startSolid )
						DebugDrawText( hullTrace.endPos, "HT startSolid", false, 0.1 )
					else
						DebugDrawText( hullTrace.endPos, "BreachPos Invalid", false, 0.1 )
				}

			}

			i++
			float nextStepDist = min( BACK_TRACE_STEP_DIST * i, BACK_TRACE_MAX_STEP )
			airTraceDist += nextStepDist
			airPos -= eyeDir * nextStepDist
		}
	}
	PerfStart( PerfIndexClient.PhaseBreach_ScorePos )
	PortalEndingSortStruct end = GetBestEnding( possibleEndings, player,eyeDir, eyePos, info )
	PerfEnd( PerfIndexClient.PhaseBreach_ScorePos )
	return info
}


PhaseBreachTargetInfo function GetPhaseBreachTargetInfo_OLD( entity player )
{
	PhaseBreachTargetInfo info
	info.startPos   = player.GetOrigin()
	info.finalPos   = player.GetOrigin()
	info.startCrouched = player.IsCrouched()

	vector eyePos = player.EyePosition()
	vector eyeDir = player.GetViewVector()
	eyeDir          = Normalize( eyeDir )

	vector mins = player.GetPlayerMins()
	vector maxs = player.GetPlayerMaxs()

	
	if ( !file.allowStartOnMovers )
	{
		entity groundEnt = player.GetGroundEntity()
		if ( GetPusherEnt( groundEnt ) )
			return info
	}

	
	
	if ( ! PhaseTunnel_IsPortalExitPointValid( player, info.startPos, player, true, info.startCrouched,DEBUG_DRAW_TARGETING ) )
	{
		info.startBlocked = true
		return info
	}

	float rangeNormal = file.maxDist
	float rangeSqr    = rangeNormal * rangeNormal

	
	float pitchClamped   = clamp( player.EyeAngles().x, -file.maxAngleForFullDist, file.maxAngleForFullDist )
	float rangeEffective = rangeNormal / deg_cos( pitchClamped )

	array<entity> ignoredEnts = [ player ]

	
	PhaseBreachTraceResults eyeTrace = DoEyeTrace( eyePos, eyeDir, rangeEffective, ignoredEnts, mins, maxs )
	if ( eyeTrace.foundValidEnd && IsBreachPositionValid( player, eyeTrace.adjustedEndPos, eyeTrace.results.hitEnt, eyeDir, eyeTrace.results.surfaceNormal ) )
	{
		bool success = GenerateBreachPathInfo( player, info, eyeTrace.adjustedEndPos )

		if ( success )
		{
			info.portalQuality = FLT_MAX
			return info
		}
	}

	array<vector> possibleEndings

	
	{
		if ( eyeTrace.results.fraction < 1.0 )
		{
			vector ledgeTraceEndPos     = eyeTrace.results.endPos + 32 * Normalize( <eyeDir.x, eyeDir.y, 0> ) - <0, 0, 24>
			vector aboveLedgeEndPos     = ledgeTraceEndPos + <0, 0, 60>

			
			TraceResults eyeToAboveLedgeTrace = TraceHull( eyePos,aboveLedgeEndPos, <-5,-5,-5>, <5,5,5>, ignoredEnts, TRACE_MASK_ABILITY_HULL, TRACE_COLLISION_GROUP_PLAYER )
			if ( eyeToAboveLedgeTrace.fraction == 1.0 )
			{
				TraceResults ledgeDownTrace = TraceHull( aboveLedgeEndPos, ledgeTraceEndPos, <-5,-5,-5>, <5,5,5>, ignoredEnts, TRACE_MASK_ABILITY_HULL, TRACE_COLLISION_GROUP_PLAYER )
				DrawDebugSphereIfDebugging( ledgeDownTrace.endPos, 150, 150, 0 )

				if ( ledgeDownTrace.fraction < 1.0 )
				{
					TraceResults hullTrace = DoHullTraceForExit( mins, maxs, ledgeDownTrace.endPos )

					if ( !hullTrace.startSolid && IsBreachPositionValid( player, hullTrace.endPos, hullTrace.hitEnt != null ? hullTrace.hitEnt : ledgeDownTrace.hitEnt, eyeDir, ledgeDownTrace.surfaceNormal ) )
					{
						possibleEndings.append( hullTrace.endPos )
						if ( DEBUG_DRAW_TARGETING )
							DebugDrawText(hullTrace.endPos, "Old1", false, 0.1 )
					}
				}
			}
		}
	}

	
	{
		PhaseBreachTraceResults lowerEyeTrace = DoEyeTrace( eyePos, VectorRotateAxis( eyeDir, player.GetRightVector(), -1 ), rangeEffective, ignoredEnts, player.GetPlayerMins(), player.GetPlayerMaxs() )

		if ( lowerEyeTrace.results.fraction < 1.0 )
		{
			vector ledgeTraceEndPos     = lowerEyeTrace.results.endPos + 24 * Normalize( <eyeDir.x, eyeDir.y, 0> ) - <0, 0, 24>
			vector aboveLedgeEndPos     = ledgeTraceEndPos + <0, 0, 60>

			
			TraceResults eyeToAboveLedgeTrace = TraceHull( eyePos,aboveLedgeEndPos, <-5,-5,-5>, <5,5,5>, ignoredEnts, TRACE_MASK_ABILITY_HULL, TRACE_COLLISION_GROUP_PLAYER )
			if ( eyeToAboveLedgeTrace.fraction == 1.0 )
			{
				TraceResults ledgeDownTrace = TraceHull( ledgeTraceEndPos + <0, 0, 60>, ledgeTraceEndPos, <-5,-5,-5>, <5,5,5>,ignoredEnts, TRACE_MASK_ABILITY_HULL, TRACE_COLLISION_GROUP_PLAYER )
				DrawDebugSphereIfDebugging( ledgeDownTrace.endPos, 150, 150, 0 )

				if ( ledgeDownTrace.fraction < 1.0 )
				{
					TraceResults hullTrace = DoHullTraceForExit( mins, maxs, ledgeDownTrace.endPos )

					if ( !hullTrace.startSolid && IsBreachPositionValid( player, hullTrace.endPos, hullTrace.hitEnt != null ? hullTrace.hitEnt : ledgeDownTrace.hitEnt, eyeDir, ledgeDownTrace.surfaceNormal ) )
					{
						possibleEndings.append( hullTrace.endPos )
						if ( DEBUG_DRAW_TARGETING )
							DebugDrawText(hullTrace.endPos, "Old2", false, 0.1 )
					}
				}
			}
		}
	}

	
	{
		TraceResults downTrace = TraceHull( eyeTrace.adjustedEndPos, eyeTrace.adjustedEndPos - <0.0, 0.0, DOWN_TRACE_DISTANCE>, <-5,-5,-5>, <5,5,5>, ignoredEnts, TRACE_MASK_ABILITY_HULL, TRACE_COLLISION_GROUP_PLAYER )
		DrawDebugSphereIfDebugging( downTrace.endPos, 0, 255, 0 )

		if ( downTrace.fraction < 1.0 )
		{
			TraceResults hullTrace = DoHullTraceForExit( mins, maxs, downTrace.endPos )

			if ( !hullTrace.startSolid && IsBreachPositionValid( player, hullTrace.endPos, hullTrace.hitEnt != null ? hullTrace.hitEnt : downTrace.hitEnt, eyeDir, downTrace.surfaceNormal ) )
			{
				possibleEndings.append( hullTrace.endPos )
				if ( DEBUG_DRAW_TARGETING )
					DebugDrawText(hullTrace.endPos, "Olddown1", false, 0.1 )
			}
		}
	}

	
	{
		vector airPos      = eyeTrace.adjustedEndPos - eyeDir * BACK_TRACE_STEP_DIST
		float airTraceDist = BACK_TRACE_STEP_DIST
		int i              = 0
		while ( airTraceDist < (rangeEffective - 250.0) && (DotProduct( eyeTrace.adjustedEndPos - eyePos, airPos - eyePos ) > 0) )
		{
			TraceResults airDownTrace = TraceHull( airPos, airPos - <0.0, 0.0, DOWN_TRACE_DISTANCE>, <-5,-5,-5>, <5,5,5>, ignoredEnts, TRACE_MASK_ABILITY_HULL, TRACE_COLLISION_GROUP_PLAYER )
			DrawDebugSphereIfDebugging( airPos, 0, 255, 255 )
			DrawDebugSphereIfDebugging( airDownTrace.endPos, 255, 0, 255 )
			if ( airDownTrace.fraction < 1.0 )
			{
				TraceResults hullTrace = DoHullTraceForExit( mins, maxs, airDownTrace.endPos )

				if ( !hullTrace.startSolid && IsBreachPositionValid( player, hullTrace.endPos, hullTrace.hitEnt != null ? hullTrace.hitEnt : airDownTrace.hitEnt, eyeDir, airDownTrace.surfaceNormal ) )
				{
					possibleEndings.append( hullTrace.endPos )
					if ( DEBUG_DRAW_TARGETING )
						DebugDrawText(hullTrace.endPos, "Olddownset", false, 0.1 )
				}
			}

			i++
			float nextStepDist = min( BACK_TRACE_STEP_DIST * i, BACK_TRACE_MAX_STEP )
			airTraceDist += nextStepDist
			airPos -= eyeDir * nextStepDist
		}
	}

	PortalEndingSortStruct end = GetBestEnding( possibleEndings, player,eyeDir, eyePos, info )
	return info
}


PhaseBreachTraceResults function DoEyeTrace( vector eyePos, vector eyeDir, float effectiveRange, array<entity> ignoredEntities, vector mins, vector maxs )
{
	PhaseBreachTraceResults eyeTraceResults

	
	TraceResults initialTrace = TraceHull( eyePos, eyePos + (eyeDir * effectiveRange), <-5,-5,-5>, <5,5,5>, ignoredEntities, TRACE_MASK_ABILITY_HULL, TRACE_COLLISION_GROUP_PLAYER )
	eyeTraceResults.adjustedEndPos = initialTrace.endPos
	eyeTraceResults.results        = initialTrace

	DrawDebugSphereIfDebugging( initialTrace.endPos, 255, 0, 0 )

	if ( initialTrace.fraction < 1.0 )
	{
		if ( IsNormalVertical( initialTrace.surfaceNormal ) )
			eyeTraceResults.foundValidEnd = true
		else
			eyeTraceResults.adjustedEndPos = initialTrace.endPos - eyeDir * 30.0

		DrawDebugSphereIfDebugging( eyeTraceResults.adjustedEndPos, 150, 0, 0 )
	}

	if ( eyeTraceResults.foundValidEnd )
	{
		TraceResults hullTrace = DoHullTraceForExit( mins, maxs, eyeTraceResults.adjustedEndPos )

		if ( hullTrace.startSolid )
			eyeTraceResults.foundValidEnd = false
		else
			eyeTraceResults.adjustedEndPos = hullTrace.endPos
	}

	return eyeTraceResults
}


bool function IsBreachPositionValid( entity player, vector position, entity traceHitEnt, vector eyeDir, vector normal )
{
	if ( IsValid( traceHitEnt ) )
	{
		if ( traceHitEnt.IsPlayer() || traceHitEnt.IsNPC() || IsDeathboxFlyer( traceHitEnt ) )
			return false

		if ( traceHitEnt.GetScriptName() == CRYPTO_DRONE_SCRIPTNAME  )
			return false

		if ( traceHitEnt.IsProjectile() )
			return false

		entity pusher = GetPusherEnt( traceHitEnt )
		if ( pusher )
		{
			if ( ! file.allowEndOnMovers )
				return false

			if ( DEBUG_DRAW_PUSHER_MOVEMENT )
			{
				vector pusherVelAtPoint = pusher.GetAbsVelocityAtPoint(position)
				DebugDrawScreenText( 0.1,0.6, "Pusher " + pusher + ", speed is " + Length(pusherVelAtPoint) + " , vel is " + pusherVelAtPoint )
			}

			if ( LengthSqr(pusher.GetAbsVelocityAtPoint(position)) > file.maxEndingMoverSpeedSqr )	
				return false
		}
	}

	vector eyePos = player.EyePosition()
	if ( DotProduct( eyeDir, Normalize( position - eyePos ) ) < PHASE_BREACH_MIN_VIEW_DOT )
		return false

	if ( !IsNormalVertical( normal ) )
		return false

	foreach ( entity trigger in GetTriggersByClassesInRealms_HullSize(
		file.invalidTriggerEndingTypes,
		position, position,
		player.GetRealms(), TRACE_MASK_PLAYERSOLID,
		player.GetPlayerMins(), player.GetPlayerMaxs() ) )
	{
		return false
	}

	TraceResults eyeToDownTrace = TraceLine( eyePos, position + <0, 0, 48.0>, [player], TRACE_MASK_ABILITY, TRACE_COLLISION_GROUP_PLAYER )
	if ( eyeToDownTrace.fraction < 1.0 )
		return false

	return true
}


TraceResults function DoHullTraceForExit( vector mins, vector maxs, vector pos, float zClearance = 12.0 )
{
	TraceResults hullTrace = TraceHull( pos + <0, 0, zClearance>, pos, mins, maxs, null, TRACE_MASK_PLAYERSOLID, TRACE_COLLISION_GROUP_NONE )
	
	return hullTrace
}

struct PortalEndingSortStruct
{
	vector pos
	float  val = -1.0
}

PortalEndingSortStruct function GetBestEnding( array<vector> possibleEndings, entity player, vector eyeDir, vector eyePos, PhaseBreachTargetInfo info )
{
	PortalEndingSortStruct failedEnding

	array<PortalEndingSortStruct> endings
	foreach ( vector pos in possibleEndings )
	{
		PortalEndingSortStruct end
		end.pos = pos

		end.val = ScoreEndPosition( pos, eyeDir, eyePos, DEBUG_DRAW_ENDING_SCORES )

		endings.append( end )
	}

	endings.sort( SortEndingStruct )



	foreach ( ending in endings )
	{
		bool success = GenerateBreachPathInfo( player, info, ending.pos )

		if ( success )
		{
			info.portalQuality = ending.val
			return ending
		}
		else
		{
			if ( DEBUG_DRAW_ENDING_SCORES )
				DebugDrawText( ending.pos + <0,0,5>, "No path", false, 0.1 )
		}
	}

	PortalEndingSortStruct emptyEnding
	return emptyEnding
}

float function ScoreEndPosition( vector pos, vector eyeDir, vector eyePos, bool debugDraw = false )
{
	vector dirToPortal 	= pos - eyePos
	float distance 		= Length( dirToPortal )

	float dot = DotProduct( eyeDir, dirToPortal / distance )

	const float WEIGHT_DOT = 75
	const float WEIGHT_DIST = 60

	float dotScore = GraphCapped( dot, PHASE_BREACH_MIN_VIEW_DOT, 1.0, 0, WEIGHT_DOT )

	float distScore = GraphCapped( distance, 0, file.maxDist, 0,   WEIGHT_DIST)

	float totalScore = dotScore + distScore

	
	
	

	if ( debugDraw )
	{
		string scoreText = string( totalScore ) + "\nDotScore: " + dotScore + "\nDistScore: " + distScore

		DebugDrawText( pos + <0,0,-5>, scoreText, false, 0.1 )
	}

	return totalScore
}


int function SortEndingStruct( PortalEndingSortStruct a, PortalEndingSortStruct b )
{
	if ( a.val > b.val )
		return -1
	else if ( b.val > a.val )
		return 1

	return 0
}


bool function GenerateBreachPathInfo( entity player, PhaseBreachTargetInfo info, vector endPos )
{
	
	
	if ( !PhaseTunnel_IsPortalExitPointValid( player, endPos, player, true, false, DEBUG_DRAW_TARGETING ) )
		return false

	PerfStart( PerfIndexClient.PhaseBreach_ScorePos_Generate )
	vector portalDir = Normalize( endPos - info.startPos )
	float distCheck  = Distance( info.startPos, endPos )

	if ( info.posList.len() > 0 )
		info.posList.clear()

	info.posList.append( info.startPos )

	while ( info.pathDistance < distCheck )
	{
		float step = min( TUNNEL_STEP_DIST, distCheck - info.pathDistance )

		vector newPos = info.startPos + (portalDir * step)
		info.pathDistance += step
		info.posList.append( newPos )
		info.startPos     = newPos
	}

	int posListCount = info.posList.len()
	info.posList[ posListCount - 1 ] = endPos
	info.finalPos   = endPos

	bool successful = (posListCount > 2) && info.pathDistance > 200.0


		if ( successful )
			DrawDebugSphereIfDebugging( info.finalPos, 0, 0, 255 )

	PerfEnd( PerfIndexClient.PhaseBreach_ScorePos_Generate )
	return successful
}

void function PhaseBreachCrosshair_Thread( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	player.EndSignal( "OnDeath" )

	weapon.EndSignal( SIGNAL_PHASE_BREACH_STOP_PLACEMENT )
	weapon.EndSignal( "OnDestroy" )

	while (true)
	{
		vector traceStart = player.EyePosition()
		vector traceEnd = player.EyePosition() + player.GetPlayerOrNPCViewVector() * (file.maxDist-15)
		TraceResults tr = TraceLine( traceStart, traceEnd , [ player ], TRACE_MASK_SOLID_BRUSHONLY , TRACE_COLLISION_GROUP_PLAYER, player )
		int pointInRange = tr.fraction < 1.0 ? 1 : -1

		if ( IsValid( weapon ) )
		{
			bool isPredictableEnt = false

				isPredictableEnt = weapon.GetPredictable()




			if ( isPredictableEnt )
				weapon.SetScriptInt0( pointInRange )
		}

		WaitFrame()
	}

}


void function DrawDebugSphereIfDebugging( vector origin, int r, int g, int b )
{
#if DEV
		if ( DEBUG_DRAW_PLACEMENT_TRACES )
			DebugDrawSphere( origin, 5.0, <r, g, b>, false, 0.1 )
#endif
}



void function PhaseBreachPlacement_Thread( entity weapon )
{
	Signal( weapon, SIGNAL_PHASE_BREACH_STOP_PLACEMENT )

	weapon.EndSignal( SIGNAL_PHASE_BREACH_STOP_PLACEMENT )
	weapon.EndSignal( "OnDestroy" )

	entity player = weapon.GetWeaponOwner()
	player.EndSignal( "OnDeath" )

	OnThreadEnd(
		function() : ()
		{
			if ( EffectDoesExist( file.targetingFxHandle ) )
				EffectStop( file.targetingFxHandle, true, true )
			if ( EffectDoesExist( file.targetingFxHandleDir ) )
				EffectStop( file.targetingFxHandleDir, true, true )
			if ( EffectDoesExist( file.targetingInvalidFxHandle ) )
				EffectStop( file.targetingInvalidFxHandle, true, true )

			if ( file.targetingHint != "'" )
				HidePlayerHint( file.targetingHint )
		}
	)

	int fxID		= GetParticleSystemIndex( BREACH_TARGET_FX )
	
	int wallDownFXID	= GetParticleSystemIndex(BREACH_FX_AR_DIR )

















	while ( true )
	{
		if ( !IsValid( player ) )
			return

		PhaseBreachTargetInfo info
		if ( player in file.portalTargetTable )
			info = file.portalTargetTable[ player ]
		else
		{











			info = GetPhaseBreachTargetInfo_OLD( player  )


		}

		if ( info.portalQuality > 0 )
		{
			if ( !EffectDoesExist( file.targetingFxHandle ) )
			{
				file.targetingFxHandle = StartParticleEffectInWorldWithHandle( fxID, info.finalPos, <0,0,0> )
				EffectSetControlPointVector( file.targetingFxHandle, 1, TEAM_COLOR_FRIENDLY )
			}

			EffectSetControlPointVector( file.targetingFxHandle, 0, info.finalPos )


































			
			
			
			


			HidePlayerHint( file.targetingHint )
			file.targetingHint = ""
		}
		else
		{
			if ( EffectDoesExist( file.targetingFxHandle ) )
			{
				EffectStop( file.targetingFxHandle, true, false )
			}
			if ( EffectDoesExist( file.targetingFxHandleDir ) )
			{
				EffectStop( file.targetingFxHandleDir, true, false )
			}

			
			
			
			
			
			
			
			
			
			
			
			


			file.targetingHint = PLACEMENT_FAILED_HINT
			AddPlayerHint( 60.0, 0, $"", file.targetingHint )
		}

		WaitFrame()
	}
}




#if DEV

void function DEV_ValidateAgainstOldTargeting( entity player, PhaseBreachTargetInfo info )
{
	PerfStart( PerfIndexClient.PhaseBreach_GetPosition_OLD )
	PhaseBreachTargetInfo oldInfo = GetPhaseBreachTargetInfo_OLD( player )
	PerfEnd( PerfIndexClient.PhaseBreach_GetPosition_OLD )

	if ( LOG_VALIDATION_DATA )
	{
		file.numTargetingRuns++

		if ( info.portalQuality > 0 )
			file.newTargetingHits++

		if ( oldInfo.portalQuality > 0 )
			file.oldTargetingHits++

		
		if ( info.portalQuality == 0 && oldInfo.portalQuality > 0 )
		{
			if ( DEBUG_DRAW_VALIDATION )
			{
				Warning("Ash Ult: Old algorithm found a point and the new one failed to at all.")
				DebugDrawSphere( oldInfo.finalPos, 20, COLOR_RED, false, 0.1 )
			}
			if ( DEV_LogValidationCase( player, file.oldTargetingWins ) )
			{
				
			}
		}

		
		if ( info.portalQuality > 0 && oldInfo.portalQuality == 0 )
		{
			DEV_LogValidationCase( player, file.newTargetingWins, false )
		}

		
		if ( info.portalQuality > 0 && oldInfo.portalQuality > 0 )
		{
			vector eyePos = player.EyePosition()
			vector eyeDir = player.GetViewVector()
			float newScore = ScoreEndPosition(info.finalPos, eyeDir, eyePos )
			float oldScore = ScoreEndPosition(oldInfo.finalPos, eyeDir, eyePos )

			float newOldDistance = Distance( info.finalPos, oldInfo.finalPos )

			vector eyeToNew = info.finalPos - eyePos
			vector eyeToOld = oldInfo.finalPos - eyePos

			float newOldDot = DotProduct( Normalize(eyeToNew), Normalize(eyeToOld) )
			if ( newOldDistance > 10 * METERS_TO_INCHES && newOldDot < DOT_4DEGREE)
			{
				
					


				if ( oldScore > newScore )
				{
					if ( DEBUG_DRAW_VALIDATION )
					{
						Warning( "Ash Ult: Old algorithm found a point which scored better than the new one. Distance between them is " + newOldDistance + ". Dot between " + newOldDot + ". newDotScore: " + newScore + " oldDotScore: " + oldScore )
						DebugDrawSphere( oldInfo.finalPos, 20, COLOR_RED, false, 0.1 )
					}
					DEV_LogValidationCase( player, file.oldTargetingBetter )
				}
			}

			file.newAvgScore = (file.newAvgScore * (file.numTargetingRuns-1)/file.numTargetingRuns) + (newScore/file.numTargetingRuns)
			file.oldAvgScore = (file.oldAvgScore * (file.numTargetingRuns-1)/file.numTargetingRuns) + (oldScore/file.numTargetingRuns)

		}

		if ( DEBUG_DRAW_VALIDATION )
		{
			float newSuccessRate = float(file.newTargetingHits) / float(file.numTargetingRuns) * 100.0
			float oldSuccessRate = float(file.oldTargetingHits) / float(file.numTargetingRuns) * 100.0

			string debugText = "Ash targeting accuracy:" +
			"\n New Success Rate " + newSuccessRate +
			"\n Old Success Rate " + oldSuccessRate +
			"\n New Avg Score " + file.newAvgScore +
			"\n Old Avg Score " + file.oldAvgScore +
			"\n New Wins " + file.newTargetingWins.len() +
			"\n Old Wins " + file.oldTargetingWins.len() +
			"\n Old better " + file.oldTargetingBetter.len()


			DebugDrawScreenText( 0.1, 0.3, debugText )
			DebugDrawScreenTextWithColor( 0.1, 0.28, "ASH VALIDATION ON", COLOR_LIGHT_RED )
		}
	}
}

bool function DEV_LogValidationCase( entity player, table<int,string> validationList, bool isBad = true )
{
	vector playerPos       = player.GetOrigin()
	vector playerEyeAngles = player.EyeAngles()

	string setPosString = "setpos " + playerPos.x + " " + playerPos.y + " " + playerPos.z + "; setang " + playerEyeAngles.x + " " + playerEyeAngles.y + " " + playerEyeAngles.z
	string setPosStringSimple = "setpos " + int(playerPos.x) + " " + int(playerPos.y) + " " + int(playerPos.z) + "; setang " + int(playerEyeAngles.x) + " " + int(playerEyeAngles.y) + " " + int(playerEyeAngles.z)

	int setPosHash = StringHash( setPosStringSimple )

	if ( !(setPosHash in validationList ) )
	{
		validationList[setPosHash] <- setPosString
		return true
	}
	if ( isBad )
		printt( setPosString )
	return false
}

void function DEV_ToggleAshValidation()
{
	DEV_DO_VALIDATION = !DEV_DO_VALIDATION
}
void function DEV_ClearTargetingData()
{
	file.numTargetingRuns = 0
	file.newTargetingHits = 0
	file.oldTargetingHits = 0

	file.newTargetingWins.clear()
	file.oldTargetingWins.clear()
	file.oldTargetingBetter.clear()

	file.newAvgScore = 0.0
	file.oldAvgScore = 0.0
}


#endif
