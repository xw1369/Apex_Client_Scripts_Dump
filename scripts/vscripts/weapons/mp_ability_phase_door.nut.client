
global function MpAbilityPhaseDoor_Init
global function OnWeaponActivate_ability_phase_door
global function OnWeaponDeactivate_ability_phase_door
global function OnWeaponTossReleaseAnimEvent_ability_phase_door
global function OnProjectileCollision_ability_phase_door
global function PhaseDoor_CheckInvalidEnt


global function OnCreateClientOnlyModel_ability_phase_door


#if DEV



#endif

const float PHASE_DOOR_WARNING_DURATION = 5.0

const int PHASE_DOOR_TRIGGER_RADIUS 	= 20
const float PHASE_DOOR_PORTAL_EXTENSION_WIDTH = 2







const string PHASE_DOOR_STOP_VISUAL_EFFECT = "PhaseDoor_StopVisualEffect"



global const string PHASE_DOOR_ROOT_ENT_SCRIPTNAME = "phase_door_root_ent"
global const string PHASE_DOOR_WARMUP_ENT_SCRIPTNAME = "phase_door_warmup_ent"
global const string PHASE_DOOR_TRACE_BLOCKER_SCRIPTNAME = "alter_tac_trace_blocker"
global const string PHASE_DOOR_PORTAL_EXTENSION_SCRIPTNAME = "alter_tac_portal_extension"
global const string PHASE_DOOR_PORTAL_EXTENSION_MOVER_SCRIPTNAME = "alter_tac_portal_extension_mover"
const string PHASE_DOOR_WEAPON_NAME = "mp_ability_phase_door"

global const string PHASE_DOOR_THREAT_INDICATOR_SCRIPTNAME = "phase_door_threat"



const string PHASE_DOOR_SPAWN_SOUND = "Alter_Tac_Portal_Activate_3p"
const string PHASE_DOOR_LOOP_SOUND = "Alter_Tac_Portal_Active_3p"
const string PHASE_DOOR_WARPIN_SOUND_3P = "Alter_Tac_Portal_Enter_3p"
const string PHASE_DOOR_WARPIN_SOUND_1P = "Alter_Tac_Portal_Enter_1p"
const string PHASE_DOOR_WARPOUT_SOUND_3P = "Alter_Tac_Portal_Exit_3p"
const string PHASE_DOOR_ENDING_WARNING_SOUND = "Alter_Tac_Portal_Ending_3p"
const string PHASE_DOOR_PHASE_IN_LIFT_1P = "Alter_Tac_Portal_Lift_Enter_1p"
const string PHASE_DOOR_PHASE_IN_LIFT_3P = "Alter_Tac_Portal_Lift_Enter_3p"
const string PHASE_DOOR_ROPE_SOUND = "Alter_Tac_Portal_Lift_Base_LP"


const asset PHASE_DOOR_FX = $"P_alter_tac_portal"
const asset PHASE_DOOR_ENTER_FX = $"P_alter_tac_portal_warp_in"
const asset PHASE_DOOR_EXIT_FX = $"P_alter_tac_portal_warp"
const asset PHASE_DOOR_WARP_SCREEN_FX = $"P_alter_tac_portal_warp_screen"
const asset PHASE_DOOR_SPAWN_FX = $"P_alter_tac_portal_warmup"
const asset PHASE_DOOR_PREVIEW_FX = $"P_alter_tac_portal_preview"
const asset PHASE_DOOR_PREVIEW_EXIT_FX = $"P_alter_tac_portal_preview_exit"
const asset PHASE_DOOR_ROPE_FX = $"P_alter_tac_portal_rope"
const asset PHASE_DOOR_PLAYER_ON_ROPE_FX = $"P_alter_tac_portal_warp_rope"
const asset PHASE_DOOR_PHASED_PLAYER_FX = $"P_alter_tac_portal_warp_body"

const vector PHASE_DOOR_FX_COLOUR_FRIENDLY = <123, 159, 242>
const vector PHASE_DOOR_FX_COLOUR_ENEMY = <185, 52, 52>
const float PHASE_DOOR_FX_ENEMY_SHOW_DIST_MIN = 5 * METERS_TO_INCHES
const float PHASE_DOOR_FX_ENEMY_SHOW_DIST_MAX = 15 * METERS_TO_INCHES

const float surfaceOffset = 1.0


global const bool PHASE_DOOR_DEBUG_DRAW = false






const bool PHASE_DOOR_DEBUG_ENABLE_FF_SELF_CHECK = true

global const bool PHASE_DOOR_PORTAL_EXTENSIONS_DEBUG_DRAW = false

#if DEV
global const bool PHASE_DOOR_LOGGING = true
#else
global const bool PHASE_DOOR_LOGGING = false
#endif

enum ePhaseDoorOrientation
{
	PHASE_DOOR_ORIENTATION_WALL,
	PHASE_DOOR_ORIENTATION_CEILING_OR_FLOOR_UP 
	PHASE_DOOR_ORIENTATION_CEILING_OR_FLOOR_DOWN, 
}

struct LastPhaseDoorEnteredData
{
	entity triggerEntered
	entity triggerExited
	float enteredTime
}

struct BreachKillerChallengeConditions
{
	bool isActive
	bool knockSecured
	entity entrancePortal
}

struct
{
	
	float wallThicknessMax = 20.0 * METERS_TO_INCHES




















	bool  spawnCeilingPortalExtensions = true
	bool  spawnWallPortalExtensions = false
	float wallExtensionMinLength = 4.5
	float extensionMaxZHeight = 30
	float extensionMaxHeightNoGround = 0.0
	float ceilingPortalExtensionAngle = 60


	bool createThreatIndicator = true
	float threatIndicatorRange = 15.0
	float threatIndicatorLifetime = 1.5

} tuning

struct
{
	table<string, bool> invalidEntityScriptNames

	table<entity, float> lastEnteredPhaseDoorTime










	var placementDepthRui
	entity entrancePortalPlacementFXHolder
	entity exitPortalPlacementFXHolder
	int entrancePortalPlacementVFXHandle
	int exitPortalPlacementVFXHandle

	bool portalFXInitialized = false


	



} file

#if DEV
























































#endif

void function MpAbilityPhaseDoor_Init()
{
	SetupTuning()

	PrecacheParticleSystem( PHASE_DOOR_FX )
	PrecacheParticleSystem( PHASE_DOOR_ENTER_FX )
	PrecacheParticleSystem( PHASE_DOOR_EXIT_FX )
	PrecacheParticleSystem( PHASE_DOOR_WARP_SCREEN_FX )
	PrecacheParticleSystem( PHASE_DOOR_SPAWN_FX )
	PrecacheParticleSystem( PHASE_DOOR_PREVIEW_FX )
	PrecacheParticleSystem( PHASE_DOOR_PREVIEW_EXIT_FX )
	PrecacheParticleSystem( PHASE_DOOR_ROPE_FX )
	PrecacheParticleSystem( PHASE_DOOR_PLAYER_ON_ROPE_FX )
	PrecacheParticleSystem( PHASE_DOOR_PHASED_PLAYER_FX )

	PrecacheScriptString( PHASE_DOOR_ROOT_ENT_SCRIPTNAME )
	PrecacheScriptString( PHASE_DOOR_WARMUP_ENT_SCRIPTNAME )
	PrecacheScriptString( PHASE_DOOR_TRACE_BLOCKER_SCRIPTNAME )
	PrecacheScriptString( PHASE_DOOR_PORTAL_EXTENSION_SCRIPTNAME )
	PrecacheScriptString( PHASE_DOOR_PORTAL_EXTENSION_MOVER_SCRIPTNAME )


	RegisterSignal( PHASE_DOOR_STOP_VISUAL_EFFECT )
	AddCreateCallback( "prop_script", OnPropCreated )
	AddCreateCallback( "prop_dynamic", OnPropCreated )
	AddCallback_OnViewPlayerChanged( OnViewPlayerChanged )

	StatusEffect_RegisterEnabledCallback( eStatusEffect.phase_door_teleport_visual_effect, StartVisualEffect )
	StatusEffect_RegisterDisabledCallback( eStatusEffect.phase_door_teleport_visual_effect, StopVisualEffect )









	SetupInvalidScriptNames()

	
	if( IsBreachKillerChallengeEnabled() )
	{







	}
}

void function SetupTuning()
{
	
	















	tuning.spawnCeilingPortalExtensions = GetCurrentPlaylistVarBool( "alter_tac_spawnCeilingPortalExtensions", tuning.spawnCeilingPortalExtensions )
	tuning.spawnWallPortalExtensions 	= GetCurrentPlaylistVarBool( "alter_tac_spawnWallPortalExtensions", tuning.spawnWallPortalExtensions )
	tuning.wallExtensionMinLength       = GetCurrentPlaylistVarFloat( "alter_tac_wallExtensionMinLength", tuning.wallExtensionMinLength ) * METERS_TO_INCHES
	tuning.extensionMaxZHeight          = GetCurrentPlaylistVarFloat( "alter_tac_extensionMaxZHeight", tuning.extensionMaxZHeight ) * METERS_TO_INCHES
	tuning.extensionMaxHeightNoGround   = GetCurrentPlaylistVarFloat( "alter_tac_extensionMaxHeightNoGround", tuning.extensionMaxHeightNoGround ) * METERS_TO_INCHES
	tuning.ceilingPortalExtensionAngle  = cos( GetCurrentPlaylistVarFloat( "alter_tac_ceilingPortalExtensionAngle", tuning.ceilingPortalExtensionAngle ) * DEG_TO_RAD )


	tuning.createThreatIndicator        = GetCurrentPlaylistVarBool( "alter_tac_createThreatIndicator", tuning.createThreatIndicator )
	tuning.threatIndicatorRange         = GetCurrentPlaylistVarFloat( "alter_tac_threatIndicatorRange", tuning.threatIndicatorRange ) * METERS_TO_INCHES
	tuning.threatIndicatorLifetime      = GetCurrentPlaylistVarFloat( "alter_tac_threatIndicatorLifetime", tuning.threatIndicatorLifetime )

}

void function SetupInvalidScriptNames()
{
	file.invalidEntityScriptNames[ "jump_tower" ] <- true
	file.invalidEntityScriptNames[ "survival_door_plain" ] <- true
	file.invalidEntityScriptNames[ "GRAVITY_CANNON" ] <- true
	file.invalidEntityScriptNames[ "gravity_cannon" ] <- true
	file.invalidEntityScriptNames[ SPIDER_EGG_SCRIPT_NAME ] <- true
	file.invalidEntityScriptNames[ SPIDER_HATCHED_EGG_EDITOR_CLASS_KEYWORD ] <- true
	file.invalidEntityScriptNames[ HATCH_MDL_SCRIPTNAME ] <- true
	file.invalidEntityScriptNames[ REDEPLOY_BALLOON_INFLATABLE_SCRIPT_NAME ] <- true
	file.invalidEntityScriptNames[ CARE_PACKAGE_SCRIPTNAME ] <- true
	
	file.invalidEntityScriptNames[ HARVESTER_SCRIPTNAME ] <- true
	file.invalidEntityScriptNames[ WORKBENCH_CLUSTER_SCRIPTNAME ] <- true
	file.invalidEntityScriptNames[ WORKBENCH_SCRIPTNAME ] <- true
	file.invalidEntityScriptNames[ WORKBENCH_CLUSTER_AIRDROPPED_SCRIPTNAME ] <- true
	file.invalidEntityScriptNames[ NEXT_ZONE_SURVEY_BEACON_SCRIPTNAME ] <- true
	file.invalidEntityScriptNames[ ENEMY_SURVEY_BEACON_SCRIPTNAME ] <- true

	
	file.invalidEntityScriptNames[ ARMORED_LEAP_SHIELD_ANCHOR_SCRIPTNAME ] <- true
	file.invalidEntityScriptNames[ TROPHY_SYSTEM_NAME ] <- true

	file.invalidEntityScriptNames[ UPGRADE_CORE_CONSOLE_SCRIPT_NAME ] <- true

	
	

}

#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________WeaponFuncs___________________________(){}
#endif
void function OnWeaponActivate_ability_phase_door( entity weapon )
{

		entity player = GetLocalViewPlayer()
		if ( weapon.GetOwner() == player && file.placementDepthRui == null )
		{
			file.placementDepthRui = CreateFullscreenRui( $"ui/alter_depth_meter.rpak" )
		}
		weapon.SetSoundCodeControllerValue_ClientOverride( 0.0 )

}

void function OnWeaponDeactivate_ability_phase_door( entity weapon )
{

	entity player = weapon.GetOwner()

	if ( player != GetLocalViewPlayer() )
		return

	DeactivateClientPreviewFxRui()
	weapon.UnsetSoundCodeControllerValue_ClientOverride()

}


void function OnCreateClientOnlyModel_ability_phase_door( entity weapon, entity model, bool validHighlight )
{
	const vector COLOR_DEPTH_INVALID	= <252, 60, 45>
	const vector COLOR_DEPTH_START 		= <255, 122, 0>
	const vector COLOR_DEPTH_MID 		= <255, 210, 73>
	const vector COLOR_DEPTH_END 		= <255, 255, 255>

	vector entranceOrigin = weapon.GetObjectPlacementOrigin()
	vector entranceAngles = weapon.GetObjectPlacementAngles()
	vector exitOrigin = weapon.GetObjectPlacementSpecialOrigin()
	vector exitAngles = weapon.GetObjectPlacementSpecialAngles()

	int result = weapon.GetObjectPlacementSpecialPlacementResult()

	int fxID = GetParticleSystemIndex( PHASE_DOOR_PREVIEW_FX )
	int exitFxID = GetParticleSystemIndex( PHASE_DOOR_PREVIEW_EXIT_FX )

	if ( !IsValid( file.entrancePortalPlacementFXHolder ) )
	{
		file.entrancePortalPlacementFXHolder = CreateClientSidePropDynamic( <0,0,0>, <0,0,0>, $"mdl/dev/empty_model.rmdl" )
		file.exitPortalPlacementFXHolder = CreateClientSidePropDynamic( <0,0,0>, <0,0,0>, $"mdl/dev/empty_model.rmdl" )
	}

	vector entranceFxAngles = AnglesCompose( entranceAngles, <0,90,90> )
	vector entranceOffset = RotateVector(<2,0,0>, entranceAngles)

	vector exitFxAngles = AnglesCompose( exitAngles, <0,90,90> )
	vector exitOffset = RotateVector(<5,0,0>, exitAngles)

	file.entrancePortalPlacementFXHolder.SetOrigin( entranceOrigin + entranceOffset )
	file.entrancePortalPlacementFXHolder.SetAngles( entranceFxAngles )
	file.exitPortalPlacementFXHolder.SetOrigin( exitOrigin + exitOffset )
	file.exitPortalPlacementFXHolder.SetAngles( exitFxAngles )

	float distanceBetweenPortals = -1
	vector color = COLOR_DEPTH_INVALID
	if ( !file.portalFXInitialized )
	{
		file.entrancePortalPlacementVFXHandle = StartParticleEffectOnEntity( file.entrancePortalPlacementFXHolder, fxID, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
		file.exitPortalPlacementVFXHandle = StartParticleEffectOnEntity( file.exitPortalPlacementFXHolder, exitFxID, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )

		file.portalFXInitialized = true
	}

	distanceBetweenPortals = Distance( entranceOrigin, exitOrigin )
	if ( validHighlight )
	{
		distanceBetweenPortals = min( distanceBetweenPortals, tuning.wallThicknessMax )
		float distanceFrac = distanceBetweenPortals / tuning.wallThicknessMax
		color = GetTriLerpColor( distanceFrac, COLOR_DEPTH_END, COLOR_DEPTH_MID, COLOR_DEPTH_START, 0.6, 0.3 )
	}
	else
	{
		distanceBetweenPortals = max( distanceBetweenPortals, (tuning.wallThicknessMax + 0.02) )
	}

	if ( file.placementDepthRui == null )
	{
		file.placementDepthRui = CreateFullscreenRui( $"ui/alter_depth_meter.rpak" )
	}

	bool showExitFX = true
	bool hasTextOverride = true
	string textOverride
	switch ( result )
	{
		case OPSPR_SUCCESS:
			hasTextOverride = false
			break
		case OPSPR_TOO_FAR:
			textOverride = "#ABL_TAC_PHASE_DOOR_TOO_FAR"
			showExitFX = false
			break
		case OPSPR_TOO_DEEP:
			hasTextOverride = false
			break
		case OPSPR_ENTRANCE_BLOCKED:
		case OPSPR_ENTRANCE_INVALID_SPACE:
		case OPSPR_ENTRANCE_INVALID_OBJECT:
		case OPSPR_ENTRANCE_UNSAFE:
			textOverride = "#ABL_TAC_PHASE_DOOR_ENTRANCE_INVALID"
			showExitFX = false
			break
		case OPSPR_EXIT_BLOCKED:
		case OPSPR_EXIT_INVALID_OBJECT:
		case OPSPR_EXIT_INVALID_SPACE:
		case OPSPR_EXIT_UNSAFE:
			textOverride = "#ABL_TAC_PHASE_DOOR_EXIT_INVALID"
			showExitFX = false
			break
		case OPSPR_EXIT_NORMAL_ALIGNED:
		case OPSPR_TOO_COMPLEX:
		case OPSPR_OTHER:
			textOverride = "#ABL_TAC_PHASE_DOOR_OTHER"
			showExitFX = false
			break
	}

	if ( validHighlight )
	{
		EffectSetControlPointVector( file.entrancePortalPlacementVFXHandle, 3, PHASE_DOOR_FX_COLOUR_FRIENDLY )
		EffectSetControlPointVector( file.exitPortalPlacementVFXHandle, 3, color )
	}
	else
	{
		vector hideColour = <0,0,0>
		EffectSetControlPointVector( file.entrancePortalPlacementVFXHandle, 3, color )
		EffectSetControlPointVector( file.exitPortalPlacementVFXHandle, 3, showExitFX ? color : hideColour )
	}

	bool hasRope = false
	if ( validHighlight )
	{
		vector origin = entranceOrigin
		int portalOrientation = GetPortalDirectionForPortalExtension( AnglesToForward( entranceAngles ) )
		if ( !( portalOrientation == ePhaseDoorOrientation.PHASE_DOOR_ORIENTATION_CEILING_OR_FLOOR_DOWN ) )
		{
			origin = exitOrigin
			portalOrientation = GetPortalDirectionForPortalExtension( AnglesToForward( exitAngles ) )
		}

		PassByReferenceVector portalExtensionStartPos
		PassByReferenceVector portalExtensionEndPos
		if ( tuning.spawnCeilingPortalExtensions && (portalOrientation == ePhaseDoorOrientation.PHASE_DOOR_ORIENTATION_CEILING_OR_FLOOR_DOWN) )
		{
			hasRope = ShouldCreateVerticalPortalExtension( origin, portalExtensionStartPos, portalExtensionEndPos )
		}
	}

	float distanceRatioForSound = validHighlight ? (distanceBetweenPortals / tuning.wallThicknessMax ) * 0.9 : 1.0
	weapon.SetSoundCodeControllerValue_ClientOverride( distanceRatioForSound )

	RuiSetFloat( file.placementDepthRui, "depth", distanceBetweenPortals )
	RuiSetFloat3( file.placementDepthRui, "infoTextColorRGB", (color / 255.0) )
	RuiSetBool( file.placementDepthRui, "hasTextOverride", hasTextOverride )
	RuiSetBool( file.placementDepthRui, "hasRope", hasRope )
	if ( hasTextOverride )
	{
		RuiSetString( file.placementDepthRui, "textOverride", Localize( textOverride ) )
	}
}



void function DeactivateClientPreviewFxRui( )
{
	if ( file.placementDepthRui != null )
	{
		RuiDestroyIfAlive( file.placementDepthRui )
		file.placementDepthRui = null
	}

	if ( EffectDoesExist( file.entrancePortalPlacementVFXHandle ) )
		EffectStop( file.entrancePortalPlacementVFXHandle, true, false )

	if ( EffectDoesExist( file.exitPortalPlacementVFXHandle ) )
		EffectStop( file.exitPortalPlacementVFXHandle, true, false )

	if ( IsValid( file.entrancePortalPlacementFXHolder ) )
		file.entrancePortalPlacementFXHolder.Destroy()

	if ( IsValid( file.exitPortalPlacementFXHolder ) )
		file.exitPortalPlacementFXHolder.Destroy()

	file.portalFXInitialized = false
}



void function OnViewPlayerChanged( entity player )
{
	if ( file.placementDepthRui != null || file.portalFXInitialized )
	{
		DeactivateClientPreviewFxRui()
	}
}


var function OnWeaponTossReleaseAnimEvent_ability_phase_door( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	vector origin = weapon.GetObjectPlacementOrigin()
	vector surfaceNormal = AnglesToForward( weapon.GetObjectPlacementAngles() )
	entity entranceParent = weapon.GetObjectPlacementParent()
	vector exitOrigin = weapon.GetObjectPlacementSpecialOrigin()
	vector exitSurfaceNormal = AnglesToForward( weapon.GetObjectPlacementSpecialAngles() )
	entity exitParent = weapon.GetObjectPlacementSpecialParent()

	
	if ( !weapon.ObjectPlacementHasValidSpot() )
	{
		weapon.StartCustomActivity( "ACT_VM_MISSCENTER", WCAF_PLAYRAISEONCOMPLETE )
		return 0
	}

	entity ownerPlayer = weapon.GetWeaponOwner()

		if ( ownerPlayer == GetLocalViewPlayer() )
		{
			DeactivateClientPreviewFxRui()
		}


	weapon.EmitWeaponSound_1p3p( GetGrenadeThrowSound_1p( weapon ), GetGrenadeThrowSound_3p( weapon ) )

	bool projectilePredicted      = PROJECTILE_PREDICTED
	bool projectileLagCompensated = PROJECTILE_LAG_COMPENSATED







	vector projectileVelocity = Normalize( origin - attackParams.pos )
	projectileVelocity *= Length(attackParams.dir)
	entity projectile = Grenade_Launch( weapon, attackParams.pos, projectileVelocity, projectilePredicted, projectileLagCompensated, ZERO_VECTOR )

	PlayerUsedOffhand( ownerPlayer, weapon, true, projectile )













	int ammoReq = weapon.GetAmmoPerShot()
	return ammoReq
}

void function OnProjectileCollision_ability_phase_door( entity projectile, vector pos, vector normal, entity hitEnt, int hitBox, bool isCritical, bool isPassthrough )
{
	if ( !IsValid( projectile ) )
	{
		return
	}

	projectile.kv.solid = 0
	projectile.SetDoesExplode( false )


		

		vector forward = AnglesToForward( VectorToAngles( projectile.GetVelocity() ) )
		vector stickNormal

		if ( LengthSqr( forward ) > FLT_EPSILON )
			stickNormal = forward
		else
			stickNormal = normal

		projectile.StopPhysics()
		projectile.SetVelocity( <0, 0, 0> )
		projectile.ProjectileTeleport( pos + (projectile.proj.savedDir), AnglesCompose( VectorToAngles( stickNormal ), <90,0,0> ) )




}

#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________CreatePhaseDoor___________________________(){}
#endif








































































































































































































































































































































































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________CreatePortalExtensions___________________________(){}
#endif

int function GetPortalDirectionForPortalExtension( vector surfaceNormal )
{
	float dot = DotProduct( surfaceNormal, <0,0,1> )

	if ( dot > tuning.ceilingPortalExtensionAngle )
	{
		return ePhaseDoorOrientation.PHASE_DOOR_ORIENTATION_CEILING_OR_FLOOR_UP

	}
	else if ( dot < -tuning.ceilingPortalExtensionAngle )
	{
		return ePhaseDoorOrientation.PHASE_DOOR_ORIENTATION_CEILING_OR_FLOOR_DOWN
	}
	return ePhaseDoorOrientation.PHASE_DOOR_ORIENTATION_WALL
}

bool function ShouldCreateHorizontalPortalExtension( vector rootEntPos, vector rootEntRightVec, PassByReferenceVector portalExtensionStartPos, PassByReferenceVector portalExtensionEndPos )
{
	array<entity> ignoreArray = GetPlayerArray_AliveConnected()
	ignoreArray.extend( GetPlayerDecoyArray() )

	vector portalNormalFlattened = FlattenNormalizeVec( rootEntRightVec )
	vector startPos = rootEntPos + ( portalNormalFlattened * 32.0 )


	vector endOffset = ( portalNormalFlattened * 45.0 )
	float heightTraceFudgeValue = 5.0
	vector traceEndZPos = startPos - <0, 0, tuning.extensionMaxZHeight + heightTraceFudgeValue> + endOffset
	vector traceHorizontalStep = portalNormalFlattened * tuning.extensionMaxZHeight

	const float heightDiffForBetter = 2 * METERS_TO_INCHES

	float angleStep = DegToRad(10)
	float angle = 0
	float bestAngle = 0
	bool didHit = false
	vector endPos = traceEndZPos
	for ( int i = 0; i < 4; i++ )
	{
		TraceResults groundTrace = TraceLine( startPos, traceEndZPos + ( traceHorizontalStep * tan(angle) ), ignoreArray, TRACE_MASK_PLAYERSOLID, TRACE_COLLISION_GROUP_PLAYER_MOVEMENT )

#if PHASE_DOOR_PORTAL_EXTENSIONS_DEBUG_DRAW



			float debugDrawTime = 0

			DebugDrawArrow( startPos, traceEndZPos + ( traceHorizontalStep * tan(angle) ), 5, COLOR_BLUE, false, debugDrawTime )
#endif

		if ( groundTrace.fraction < 0.99 )
		{
			if ( !didHit || (endPos.z - groundTrace.endPos.z) > heightDiffForBetter )
			{
				endPos = groundTrace.endPos
				bestAngle = angle
			}
			didHit = true
		}

		angle += angleStep
	}

	float ziplineLength = Distance( startPos, endPos )
	if ( ziplineLength < tuning.wallExtensionMinLength )
		return false

	if ( !didHit )
	{
		if ( tuning.extensionMaxHeightNoGround == 0.0 )
			return false

		endPos = startPos - <0,0,tuning.extensionMaxHeightNoGround> + ( portalNormalFlattened * tuning.extensionMaxHeightNoGround * tan(bestAngle) ) + endOffset
	}

	portalExtensionStartPos.value = startPos
	portalExtensionEndPos.value = endPos

	return true
}

bool function ShouldCreateVerticalPortalExtension( vector rootEntPos, PassByReferenceVector portalExtensionStartPos, PassByReferenceVector portalExtensionEndPos )
{
	array<entity> ignoreArray = GetPlayerArray_AliveConnected()
	ignoreArray.extend( GetPlayerDecoyArray() )

	vector startPos = rootEntPos

	TraceResults groundTrace = TraceLine( startPos, startPos - <0,0,3000.0>, ignoreArray, TRACE_MASK_PLAYERSOLID, TRACE_COLLISION_GROUP_PLAYER_MOVEMENT )
	

#if PHASE_DOOR_PORTAL_EXTENSIONS_DEBUG_DRAW



			float debugDrawTime = 0

		DebugDrawArrow( startPos, startPos - <0,0,3000.0>, 5, COLOR_BLUE, false, debugDrawTime )
#endif

	vector endPos = groundTrace.endPos

	if ( (startPos.z - endPos.z) >= ( tuning.extensionMaxZHeight - FLT_EPSILON ) )
	{
		if ( tuning.extensionMaxHeightNoGround == 0.0 )
			return false

		endPos = startPos - <0,0,tuning.extensionMaxHeightNoGround>
	}
	else if ( (startPos.z - endPos.z) < 72 )
	{
		
		return false
	}
	else
	{
		endPos += <0,0,5>
	}

	portalExtensionStartPos.value = startPos
	portalExtensionEndPos.value = endPos

	return true
}






















































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________Teleport___________________________(){}
#endif

















bool function PortalStandardChecks( entity player )
{
	if ( player.Player_IsSkywardLaunching() )
		return false

	if ( player.Player_IsSkywardFollowing() )
		return false

	if ( player.ContextAction_IsActive() )
		return false

	if ( player.ContextAction_IsBusy() )
		return false

	if ( player.IsPlayerInAnyVehicle() )
		return false

	if ( player.Anim_IsActive() )
		return false

	if ( player.IsArmoredLeapActive() )
		return false

	return true
}
















































































































































































































































































































































































































































































































bool function PortalExtension_CanUseCallback( entity player, entity extension, int useFlags )
{
	if ( !IsValid ( player ) || !IsValid( extension ) )
		return false

	if ( !PortalStandardChecks( player ) )
		return false

	if ( StatusEffect_HasSeverity( player, eStatusEffect.placing_phase_tunnel ) )
		return false

	if ( player.Player_IsSkydiving() )
		return false

	if ( player.IsPhaseShifted() )
		return false

	if ( Bleedout_IsBleedingOut( player ) )
		return false

	if ( GetCurrentPlaylistVarBool( "alter_tac_use_better_rope_fix", true ) )
	{
		if ( StatusEffect_HasSeverity( player, eStatusEffect.phase_door_teleport_visual_effect ) )
			return false
	}
	else
	{























	}

	return true
}

#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________ClientWork___________________________(){}
#endif

void function OnPropCreated( entity ent )
{
	if ( ent.GetScriptName() == PHASE_DOOR_WARMUP_ENT_SCRIPTNAME )
	{
		ManageWarmupFX( ent )
		if ( tuning.createThreatIndicator )
		{
			thread ManageThreatIndicator_Thread( ent )
		}
	}
	if ( ent.GetScriptName() == PHASE_DOOR_ROOT_ENT_SCRIPTNAME )
	{
		ManagePortalFX( ent )






	}
	if ( ent.GetScriptName() == PHASE_DOOR_PORTAL_EXTENSION_SCRIPTNAME )
	{
		SetCallback_CanUseEntityCallback( ent, PortalExtension_CanUseCallback )
		SetCallback_ShouldUseBlockReloadCallback( ent, SimpleShouldNotBlockReloadCallback )
		thread ManageRopeSFX_Thread( ent )
	}
}

void function ManageThreatIndicator_Thread( entity portal )
{
	entity localPlayer = GetLocalViewPlayer()
	if ( !IsValid( localPlayer ) )
		return

	int team = portal.GetTeam()
	vector position = portal.GetOrigin()

	if ( IsFriendlyTeam( localPlayer.GetTeam(), team ) )
		return

#if PHASE_DOOR_DEBUG_ENABLE_FF_SELF_CHECK
	
	if ( portal.GetOwner() == localPlayer )
		return
#endif

	entity dummyProp = CreateClientSidePropDynamic( position, portal.GetAngles(), $"mdl/dev/empty_model.rmdl" )
	dummyProp.SetScriptName( PHASE_DOOR_THREAT_INDICATOR_SCRIPTNAME )

	ShowGrenadeArrow( GetLocalViewPlayer(), dummyProp, tuning.threatIndicatorRange, 0, true, eThreatIndicatorVisibility.INDICATOR_SHOW_TO_ALL )

	wait tuning.threatIndicatorLifetime

	if ( IsValid( dummyProp ) )
	{
		dummyProp.Destroy()
	}
}

void function ManageRopeSFX_Thread( entity portalExtension )
{
	portalExtension.EndSignal( "OnDestroy" )

	entity soundEnt = CreateClientSidePropDynamic( <0,0,0>, <0, 0, 0>, $"mdl/dev/empty_model.rmdl" )
	EmitSoundOnEntity( soundEnt, PHASE_DOOR_ROPE_SOUND )

	OnThreadEnd(
		function() : ( soundEnt )
		{
			if ( IsValid( soundEnt ) )
			{
				StopSoundOnEntity( soundEnt, PHASE_DOOR_ROPE_SOUND )
				soundEnt.Destroy()
			}
		}
	)

	vector extensionBottom = portalExtension.GetOrigin() + portalExtension.GetUpVector() * portalExtension.GetBoundingMins().z
	vector extensionTop = portalExtension.GetOrigin() + portalExtension.GetUpVector() * portalExtension.GetBoundingMaxs().z

	while ( true )
	{
		if ( IsValid( GetLocalViewPlayer() ) )
		{
			vector closestPointOnLine = GetClosestPointOnLineSegment( extensionTop, extensionBottom, GetLocalViewPlayer().EyePosition() )

			soundEnt.SetOrigin( closestPointOnLine )
		}

		WaitFrame()
	}
}

void function ManageWarmupFX( entity portalRootEnt )
{
	thread ManageFX_Thread( portalRootEnt, PHASE_DOOR_SPAWN_SOUND, GetParticleSystemIndex( PHASE_DOOR_SPAWN_FX ), true )
}
void function ManagePortalFX( entity portalRootEnt )
{
	thread ManageFX_Thread( portalRootEnt, PHASE_DOOR_LOOP_SOUND, GetParticleSystemIndex( PHASE_DOOR_FX ), false )
}

void function ManageFX_Thread( entity portalEnt, string sound, int fxID, bool isWarmup )
{
	entity linkedPortal = portalEnt.GetLinkEnt()

	if ( !linkedPortal )
	{
		Warning("No linked portal!")
		return
	}
	portalEnt.EndSignal( "OnDestroy" )
	linkedPortal.EndSignal( "OnDestroy" )

	entity tracingEnt = portalEnt.GetOwner()
	if ( !IsValid( tracingEnt ) )
	{
		tracingEnt = GetLocalViewPlayer()
	}

	int team = portalEnt.GetTeam()

	bool isFriendlyTeam = IsFriendlyTeam( tracingEnt.GetTeam(), team)

#if PHASE_DOOR_DEBUG_ENABLE_FF_SELF_CHECK
	if ( portalEnt.GetOwner() == GetLocalViewPlayer() )
	{
		isFriendlyTeam = true
	}
#endif

	const float FX_OFFSET = 3
	vector fxPos = portalEnt.GetOrigin() + ( FX_OFFSET * portalEnt.GetRightVector() )
	int portalFX = StartParticleEffectInWorldWithHandle( fxID, fxPos, portalEnt.GetAngles() + <0, 0, 90> )

	const float TEAMMATE_ALPHA = 0.2
	const float OBSCURED_ALPHA = 0.6

	EffectSetControlPointVector( portalFX, 3, <TEAMMATE_ALPHA,0,0> )

	if ( IsValid( tracingEnt ) )
	{
		EffectSetControlPointVector( portalFX, 2, isFriendlyTeam ? PHASE_DOOR_FX_COLOUR_FRIENDLY : PHASE_DOOR_FX_COLOUR_ENEMY )
	}

	if ( isWarmup )
	{
		EmitSoundAtPosition( TEAM_UNASSIGNED, portalEnt.GetOrigin(), sound )
	}
	else
	{
		
		SendMilesInfoForOPSPoint( portalEnt, tracingEnt, portalEnt.GetOrigin(), portalEnt.GetRightVector() )
		EmitSoundOnEntity( portalEnt, sound )
	}

	OnThreadEnd(
		function() : ( portalFX, portalEnt, sound )
		{
			StopSoundOnEntity( portalEnt, sound )
			EffectStop( portalFX, false, true )
		}
	)

	while ( true )
	{
		entity player = GetLocalViewPlayer()

		if ( !IsValid( player ) )
		{
			WaitFrame()
			continue
		}

		isFriendlyTeam = IsFriendlyTeam( player.GetTeam(), team)

#if PHASE_DOOR_DEBUG_ENABLE_FF_SELF_CHECK
		if ( portalEnt.GetOwner() == player )
		{
			isFriendlyTeam = true
		}
#endif

		if ( !isFriendlyTeam )
		{
			float alpha = 0

			float distanceToLinkedPortal = Distance( player.GetOrigin(), linkedPortal.GetOrigin() )
			if ( distanceToLinkedPortal < PHASE_DOOR_FX_ENEMY_SHOW_DIST_MAX )
			{
				if ( PlayerCanSee( player, linkedPortal, true, 180 ) )
				{
					alpha = GetValueFromFraction( distanceToLinkedPortal, PHASE_DOOR_FX_ENEMY_SHOW_DIST_MIN, PHASE_DOOR_FX_ENEMY_SHOW_DIST_MAX, OBSCURED_ALPHA, 0 )
				}
			}
			EffectSetControlPointVector( portalFX, 3, <alpha,0,0> )

			EffectSetControlPointVector( portalFX, 2, PHASE_DOOR_FX_COLOUR_ENEMY )
		}
		else
		{
			EffectSetControlPointVector( portalFX, 3, <TEAMMATE_ALPHA,0,0> )
			EffectSetControlPointVector( portalFX, 2, PHASE_DOOR_FX_COLOUR_FRIENDLY )
		}

		WaitFrame()
	}
}

void function StartVisualEffect( entity player, int statusEffect, bool actuallyChanged )
{
	if ( player != GetLocalViewPlayer() || (GetLocalViewPlayer() == GetLocalClientPlayer() && !actuallyChanged) )
		return

	if ( StatusEffect_GetSeverity( player, statusEffect ) < 1 )
		return

	EmitSoundOnEntity( player, PHASE_DOOR_WARPIN_SOUND_1P )

	thread (void function() : ( player, statusEffect ) {
		EndSignal( player, "OnDeath", PHASE_DOOR_STOP_VISUAL_EFFECT )

		int fxHandle = StartParticleEffectOnEntityWithPos( player,
			GetParticleSystemIndex( PHASE_DOOR_WARP_SCREEN_FX ),
			FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID, player.EyePosition(), <0, 0, 0> )

		EffectSetIsWithCockpit( fxHandle, true )

		OnThreadEnd( function() : ( fxHandle ) {
			CleanupFXHandle( fxHandle, false, true )
		} )

		while( true )
		{
			if ( !EffectDoesExist( fxHandle ) )
				break

			float severity = StatusEffect_GetSeverity( player, statusEffect )
			
			EffectSetControlPointVector( fxHandle, 1, <severity, 999, 0> )

			WaitFrame()
		}
	})()
}

void function StopVisualEffect( entity player, int statusEffect, bool actuallyChanged )
{
	if ( player != GetLocalViewPlayer() || (GetLocalViewPlayer() == GetLocalClientPlayer() && !actuallyChanged) )
		return

	player.Signal( PHASE_DOOR_STOP_VISUAL_EFFECT )
}


























bool function PhaseDoor_CheckInvalidEnt( entity ent )
{
	if ( IsValid( ent ) && (ent.GetScriptName() in file.invalidEntityScriptNames) )
	{
		return true
	}

	if ( IsValid( ent.GetParent() ) && (ent.GetParent().GetScriptName() in file.invalidEntityScriptNames) )
	{
		return true
	}

	return false
}

#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________BreachKillerChallenge___________________________(){}
#endif

bool function IsBreachKillerChallengeEnabled()
{
	return GetCurrentPlaylistVarBool( "alter_breach_killer_challenge_enabled", false )
}











































































































































































