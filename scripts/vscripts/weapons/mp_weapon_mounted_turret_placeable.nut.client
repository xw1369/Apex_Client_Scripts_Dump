global function MpWeaponMountedTurretPlaceable_Init

global function OnWeaponOwnerChanged_weapon_mounted_turret_placeable
global function OnWeaponAttemptOffhandSwitch_weapon_mounted_turret_placeable
global function OnWeaponPrimaryAttack_weapon_mounted_turret_placeable
global function OnWeaponActivate_weapon_mounted_turret_placeable
global function OnWeaponDeactivate_weapon_mounted_turret_placeable

global function MountedTurretPlaceable_SetEligibleForRefund


global function OnCreateClientOnlyModel_weapon_mounted_turret_placeable
global function ServerCallback_PlayTurretDestroyFX






















global enum eTurretClearUserReason
{
	TURRET_USE_FINISHED,
	TURRET_DESTROYED,
	TURRET_DISABLED,
	DEPLOYTHREADFINISHED,
	OTHERREASON,

	_count
}

const asset CAMERA_RIG = $"mdl/props/editor_ref_camera/editor_ref_camera.rmdl"

const asset MOUNTED_TURRET_PLACEABLE_MODEL = $"mdl/props/rampart_turret/rampart_turret.rmdl"
const asset MOUNTED_TURRET_PLACEABLE_SHIELD_COL_MODEL = $"mdl/fx/sentry_turret_shield.rmdl"
const asset MOUNTED_TURRET_PLACEABLE_SHIELD_FX = $"P_anti_titan_shield_3P"
global const asset COLLISION_CYLINDER_MODEL = $"mdl/props/rampart_cover_wall_replacement/rampart_cover_wall_invisible_collision_40x15_phys.rmdl"
const asset MOUNTED_TURRET_VEHICLE_COLLISION_MODEL = $"mdl/props/rampart_turret_vehicle_clip/rampart_turret_vehicle_clip_static.rmdl"

global const string MOUNTED_TURRET_PLACEABLE_WEAPON_NAME = "mp_weapon_mounted_turret_placeable"
global const string MOUNTED_TURRET_PLACEABLE_SCRIPT_NAME = "mounted_turret_placeable"
const string MOUNTED_TURRET_PLACEABLE_ENT_NAME = "rampart_turret"


const int MOUNTED_TURRET_PLACEABLE_MAX_TURRETS = 1




const float MOUNTED_TURRET_PLACEABLE_NO_SPAWN_RADIUS = 256.0
const float MOUNTED_TURRET_PLACEABLE_ICON_HEIGHT = 48.0
const int MOUNTED_TURRET_PLACEABLE_MAX_HEALTH = 350

const float TURRET_AMMO_REFUND_ON_PICKUP_FRAC = 0.5

const float EMP_DISABLE_DURATION = 15


const float MOUNTED_TURRET_PLACEABLE_PLACEMENT_RANGE_MAX = 92
const float MOUNTED_TURRET_PLACEABLE_PLACEMENT_RANGE_MIN = 32
const vector MOUNTED_TURRET_PLACEABLE_BOUND_MINS = <-8,-8,-8>
const vector MOUNTED_TURRET_PLACEABLE_BOMB_BOUND_MAXS = <8,8,8>
const vector MOUNTED_TURRET_PLACEABLE_PLACEMENT_TRACE_OFFSET = <0,0,128>
const float MOUNTED_TURRET_PLACEABLE_ANGLE_LIMIT = 0.55
const float MOUNTED_TURRET_PLACEABLE_PLACEMENT_MAX_HEIGHT_DELTA = 32.0


const bool MOUNTED_TURRET_PLACEABLE_DEBUG_DRAW_PLACEMENT = false



const FX_EMP_TURRET					= $"P_emp_body_human"
const TURRET_BASE_DESTROYED_FX		= $"P_rampart_turret_base_dest"
const TURRET_GUN_DESTROYED_FX		= $"P_rampart_turret_dest"
const TURRET_DESTROYED_GUN_ATTACH	= "__illumPosition"
const TURRET_DAMAGE_FX_3P			= $"P_rampart_turret_dmg"
const TURRET_PLACEABLE_RANGE_FX 	= $"P_Rampart_Turret_Range_AR"


const TURRET_DAMAGED_3P 			= "Turret_Ignite_Burn"
const MOUNT_TURRET_1P 				= "weapon_sheilaturret_mount_1p"
const MOUNT_TURRET_3P				= "weapon_sheilaturret_mount_3p"
const DISMOUNT_TURRET_3P 			= "weapon_sheilaturret_dismount_3p"



const float TURRET_DESTROYED_CALLOUT_MIN_DIST = 1024

struct MountedTurretPlaceablePlacementInfo
{
	vector origin
	vector angles
	entity parentTo
	bool success = false
}

struct MountedTurretPlaceablePlayerPlacementData
{
	vector viewOrigin	
	vector viewForward	
	vector playerOrigin 
	vector playerForward 
}

struct
{







		bool isShowingPlacementFX

	table< entity, bool > turretEligibleForRefund
	int maxNumTurretsDeployed
} file

void function MpWeaponMountedTurretPlaceable_Init()
{
	MountedTurretPlaceable_Precache()

	file.maxNumTurretsDeployed = GetCurrentPlaylistVarInt( "rampart_max_turrets_deployed", MOUNTED_TURRET_PLACEABLE_MAX_TURRETS )

	Remote_RegisterServerFunction( "ClientCallback_TryPickupMountedTurret", "typed_entity", "turret" )

	RegisterSignal( "EnterMountedTurret" )











		
		ModelFX_BeginData( "turretDamage", MOUNTED_TURRET_PLACEABLE_MODEL, "all", true )
		ModelFX_AddTagHealthFX( 0.50, "__illumPosition", TURRET_DAMAGE_FX_3P, false )
		ModelFX_EndData()

		RegisterConCommandTriggeredCallback( "+scriptCommand5", OnCharacterButtonPressed )
		AddCallback_UseEntGainFocus( MountedTurretPlaceable_OnGainFocus )
		AddCallback_UseEntLoseFocus( MountedTurretPlaceable_OnLoseFocus )

		RegisterMinimapPackage( "prop_script", eMinimapObject_prop_script.RAMPART_TURRET, MINIMAP_OBJECT_RUI, MinimapPackage_RampartGun, FULLMAP_OBJECT_RUI, MinimapPackage_RampartGun )

}














void function MountedTurretPlaceable_Precache()
{
	RegisterSignal( "MountedTurretPlaceable_PickedUp" )
	RegisterSignal( "MountedTurretPlaceable_Active" )
	RegisterSignal( "MountedTurretPlaceable_PlayerLeave" )

	PrecacheScriptString( MOUNTED_TURRET_PLACEABLE_SCRIPT_NAME )
	PrecacheScriptString( DISABLE_WAYPOINT_SCRIPTNAME )

	PrecacheModel( MOUNTED_TURRET_PLACEABLE_MODEL )
	PrecacheModel( MOUNTED_TURRET_PLACEABLE_SHIELD_COL_MODEL )
	PrecacheModel( CAMERA_RIG )
	PrecacheModel( COLLISION_CYLINDER_MODEL )
	PrecacheModel( MOUNTED_TURRET_VEHICLE_COLLISION_MODEL )

	PrecacheParticleSystem( MOUNTED_TURRET_PLACEABLE_SHIELD_FX )
	PrecacheParticleSystem( TURRET_DAMAGE_FX_3P )
	PrecacheParticleSystem( TURRET_BASE_DESTROYED_FX )
	PrecacheParticleSystem( TURRET_GUN_DESTROYED_FX )
	PrecacheParticleSystem( TURRET_PLACEABLE_RANGE_FX )






		RegisterSignal( "MountedTurretPlaceable_StopPlacementProxy" )

		AddCreateCallback( "turret", MountedTurretPlaceable_OnTurretCreated )
		AddDestroyCallback( "turret", MountedTurretPlaceable_OnTurretDestroyed )

}

void function OnWeaponOwnerChanged_weapon_mounted_turret_placeable( entity weapon, WeaponOwnerChangedParams changeParams )
{
	entity weaponOwner = weapon.GetWeaponOwner()

	if ( !IsValid( weaponOwner ) )
		return

	if ( !weaponOwner.IsPlayer() )
		return
}


void function OnCreateClientOnlyModel_weapon_mounted_turret_placeable( entity weapon, entity model, bool validHighlight )
{
	if ( validHighlight )
	{
		DeployableModelHighlight( model )
		if (!file.isShowingPlacementFX)
			thread ShowPlacementFX( weapon, model )
	}
	else
	{
		DeployableModelInvalidHighlight( model )
	}
}

void function ShowPlacementFX( entity weapon, entity model )
{
	if ( !IsValid(weapon.GetOwner()) )
		return

	weapon.GetOwner().EndSignal( "MountedTurretPlaceable_StopPlacementProxy" )

	file.isShowingPlacementFX = true

	
	

	OnThreadEnd(
		function() : (  )
		{
			file.isShowingPlacementFX = false

			
			
		}
	)

	WaitForever()
}

void function ServerCallback_PlayTurretDestroyFX( vector baseOrigin, vector baseAngles, vector gunOrigin, vector gunAngles )
{
	int baseFxID = GetParticleSystemIndex( TURRET_BASE_DESTROYED_FX )
	int gunFxID = GetParticleSystemIndex( TURRET_GUN_DESTROYED_FX )

	StartParticleEffectInWorld( baseFxID, baseOrigin, baseAngles )
	StartParticleEffectInWorld( gunFxID, gunOrigin, gunAngles )
}


void function OnWeaponActivate_weapon_mounted_turret_placeable( entity weapon )
{
	entity ownerPlayer = weapon.GetWeaponOwner()
	weapon.w.startChargeTime = Time()

	Assert( ownerPlayer.IsPlayer() )

		if ( ownerPlayer != GetLocalViewPlayer() )
			return

		AddPlayerHint( 120, 0, $"", "#WPN_MOUNTED_TURRET_PLAYER_DEPLOY_HINT" )

		if ( !InPrediction() ) 
			return







}


void function OnWeaponDeactivate_weapon_mounted_turret_placeable( entity weapon )
{
	entity ownerPlayer = weapon.GetWeaponOwner()
	Assert( ownerPlayer.IsPlayer() )

		if ( ownerPlayer != GetLocalViewPlayer() )
			return

		HidePlayerHint( "#WPN_MOUNTED_TURRET_PLAYER_DEPLOY_HINT" )

		if ( !InPrediction() ) 
			return





}

var function OnWeaponPrimaryAttack_weapon_mounted_turret_placeable( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity ownerPlayer = weapon.GetWeaponOwner()
	Assert( ownerPlayer.IsPlayer() )

	
	if ( !weapon.ObjectPlacementHasValidSpot() )
	{
		weapon.DoDryfire()
		return 0
	}









		if( IsValid( GetCompassRui() ) )
			RuiSetFloat( GetCompassRui(), "turretAngle", weapon.GetObjectPlacementAngles().y )


	PlayerUsedOffhand( ownerPlayer, weapon )
	return weapon.GetAmmoPerShot()
}

bool function OnWeaponAttemptOffhandSwitch_weapon_mounted_turret_placeable( entity weapon )
{
	return true
}




















































































































































































































































































































































































































































































































































































































































































































































































































bool function MountedTurretPlaceable_CanUse( entity player, entity ent, int useFlags )
{
	if ( IsValid( ent.GetDriver() ) )
	{
		return ent.GetDriver() == player
	}

	if ( ! SURVIVAL_PlayerAllowedToPickup( player ) )
		return false

	if ( !IsTurretEnabled( ent ) )
		return false

	if ( GradeFlagsHas( ent, eGradeFlags.IS_BUSY ) )
		return false


	entity parentEnt = ent.GetParent()
	if ( EntIsHoverVehicle( parentEnt ) && HoverVehicle_IsHostileToTeam( parentEnt, player.GetTeam() ) )
		return false


	int maxAngleToAxisAllowedDegrees = 100

	vector playerEyePos = player.EyePosition()
	int attachmentIndex = ent.LookupAttachment( "turret_player_use" )

	Assert( attachmentIndex != 0 )
	vector attachmentAngles   = ent.GetAttachmentAngles( attachmentIndex )
	vector attachmentAnglesToForward  = AnglesToForward( attachmentAngles )
	vector attachmentPos = ent.GetAttachmentOrigin( attachmentIndex ) + <0,0,48> + attachmentAnglesToForward*40

	vector attachmentToPlayerEyes = Normalize( playerEyePos - attachmentPos )

	bool playerEyesInPermittedZone = DotProduct( attachmentToPlayerEyes, attachmentAnglesToForward * -1 ) > deg_cos( maxAngleToAxisAllowedDegrees )
	bool playerLookingTowardsTurretEnough = DotProduct( player.GetViewForward(), -1 * attachmentToPlayerEyes ) > deg_cos( maxAngleToAxisAllowedDegrees / 2 )

	
	TraceResults pathTraceResults = TraceLine( playerEyePos - <0,0,36>, attachmentPos, [player, ent], TRACE_MASK_SOLID, TRACE_COLLISION_GROUP_NONE )
	bool pathToTurretUnobstructed = pathTraceResults.fraction < 1.0 ? false : true

	return playerEyesInPermittedZone && playerLookingTowardsTurretEnough && pathToTurretUnobstructed

}

void function MountedTurretPlaceable_SetEligibleForRefund( entity turretProxy, bool eligible )
{
	file.turretEligibleForRefund[ turretProxy ] <- eligible
}


bool function CanReclaimTurret( entity turret )
{
	return false
}


	void function MountedTurretPlaceable_OnTurretCreated( entity ent )
	{
		switch ( ent.GetScriptName() )
		{
			case MOUNTED_TURRET_PLACEABLE_SCRIPT_NAME:
				SetCallback_CanUseEntityCallback( ent, MountedTurretPlaceable_CanUse )
				AddEntityCallback_GetUseEntOverrideText( ent, MountedTurretPlaceable_UseTextOverride )
				file.turretEligibleForRefund[ ent ] <- true
				
			break
		}
	}

	void function MountedTurretPlaceable_OnTurretDestroyed( entity ent )
	{
		if ( !IsValid( ent ) )
			return

		switch ( ent.GetScriptName() )
		{
			case MOUNTED_TURRET_PLACEABLE_SCRIPT_NAME:
				CustomUsePrompt_ClearForEntity( ent )
				break
		}
	}

	void function MountedTurretPlaceable_OnGainFocus( entity ent )
	{
		if ( !IsValid( ent ) )
			return

		if ( ent.GetScriptName() == MOUNTED_TURRET_PLACEABLE_SCRIPT_NAME )
		{
			CustomUsePrompt_Show( ent )
		}
	}

	void function MountedTurretPlaceable_OnLoseFocus( entity ent )
	{
		CustomUsePrompt_ClearForAny()
	}

	string function MountedTurretPlaceable_UseTextOverride( entity ent )
	{
		entity player = GetLocalViewPlayer()

		if ( !IsTurretEnabled( ent ) )
		{
			CustomUsePrompt_SetText( Localize("#WPN_MOUNTED_TURRET_PLACEABLE_DISABLED") )
			CustomUsePrompt_SetHintImage( $"" )
			CustomUsePrompt_ShowSourcePos( false )
		}
		else if ( !MountedTurretPlaceable_CanUse( player, ent, USE_FLAG_NONE ) || player.IsTitan() || GradeFlagsHas( ent, eGradeFlags.IS_BUSY ) )
		{
			CustomUsePrompt_SetText( Localize("#WPN_MOUNTED_TURRET_PLACEABLE_NO_INTERACTION") )
			CustomUsePrompt_SetHintImage( $"" )
			CustomUsePrompt_ShowSourcePos( false )
		}
		else if ( ent.GetOwner() == player )
		{
			CustomUsePrompt_SetSourcePos( ent.GetOrigin() + < 0, 0, 35 > )

			if ( CanReclaimTurret( ent ) )
			{
				CustomUsePrompt_SetText( Localize("#WPN_MOUNTED_TURRET_PLACEABLE_OWNER_RECLAIM") )
				CustomUsePrompt_SetAdditionalText( Localize( "#WPN_MOUNTED_TURRET_PLACEABLE_DYNAMIC" ) )
				CustomUsePrompt_SetHintImage( $"rui/hud/character_abilities/rampart_cover_pickup" )
				CustomUsePrompt_SetLineColor( <0.0, 1.0, 1.0> )
			}
			else
			{
				CustomUsePrompt_SetText( Localize("#WPN_MOUNTED_TURRET_PLACEABLE_OWNER_DESTROY") )
				CustomUsePrompt_SetAdditionalText( Localize( "#WPN_MOUNTED_TURRET_PLACEABLE_DYNAMIC" ) )
				
				CustomUsePrompt_SetHintImage( $"" )
				CustomUsePrompt_SetLineColor( <1.0, 0.5, 0.0> )
			}


			if ( PlayerIsInADS( player ) )
				CustomUsePrompt_ShowSourcePos( false )
			else
				CustomUsePrompt_ShowSourcePos( true )
		}
		else
		{
			CustomUsePrompt_SetSourcePos( ent.GetOrigin() + < 0, 0, 35 > )
			CustomUsePrompt_ShowSourcePos( true )
			CustomUsePrompt_SetText( Localize( "#WPN_MOUNTED_TURRET_PLACEABLE_DYNAMIC" ) )
		}

		return ""
	}

	void function OnCharacterButtonPressed( entity player )
	{
		entity useEnt = player.GetUsePromptEntity()
		if ( !IsValid( useEnt ) || useEnt.GetScriptName() != MOUNTED_TURRET_PLACEABLE_SCRIPT_NAME )
			return

		if ( useEnt.GetOwner() != player )
			return

		Remote_ServerCallFunction( "ClientCallback_TryPickupMountedTurret", useEnt )
	}

	void function MountedTurretPlaceable_CreateHUDMarker( entity turret )
	{
		entity localClientPlayer = GetLocalClientPlayer()

		turret.EndSignal( "OnDestroy" )

		if ( !MountedTurretPlaceable_ShouldShowIcon( localClientPlayer, turret ) )
			return

		vector pos = turret.GetOrigin() + <0,0,MOUNTED_TURRET_PLACEABLE_ICON_HEIGHT>
		var rui = CreateCockpitRui( $"ui/cover_wall_marker_icons.rpak", RuiCalculateDistanceSortKey( localClientPlayer.EyePosition(), pos ) )
		RuiTrackFloat( rui, "healthFrac", turret, RUI_TRACK_HEALTH )
		RuiTrackFloat3( rui, "pos", turret, RUI_TRACK_OVERHEAD_FOLLOW )
		RuiKeepSortKeyUpdated( rui, true, "pos" )

		OnThreadEnd(
		function() : ( rui )
		{
			RuiDestroy( rui )
		}
		)

		WaitForever()
	}

	bool function MountedTurretPlaceable_ShouldShowIcon( entity localPlayer, entity wall )
	{
		if ( !GamePlayingOrSuddenDeath() )
			return false

		
		
		entity owner = wall.GetOwner()
		if ( !IsValid( owner ) )
			return false

		if ( localPlayer.GetTeam() != owner.GetTeam() )
			return false

		return true
	}


bool function IsTurretEnabled( entity turret )
{









		foreach ( entity linkedEnt in turret.GetLinkEntArray() )
		{
			if ( IsPlayerWaypoint( linkedEnt ) && linkedEnt.GetWaypointType() == eWaypoint.DEVICE_DISABLED )
				return false
		}

		return true

}

#if 0
entity function MountedTurretPlaceable_CreateProxyModel( asset modelName )
{



		entity proxy = CreateClientSidePropDynamic( <0,0,0>, <0,0,0>, modelName )

	proxy.kv.renderamt = 255
	proxy.kv.rendermode = 3
	proxy.kv.rendercolor = "255 255 255 255"
	proxy.Hide()

	return proxy
}

MountedTurretPlaceablePlacementInfo function MountedTurretPlaceable_GetPlacementInfo( entity player, entity turretModel )
{
	vector eyePos = player.EyePosition()
	vector viewVec = player.GetViewVector()
	vector angles = < 0, VectorToAngles( viewVec ).y, 0 >
	viewVec = AnglesToForward( angles )

	float maxRange = MOUNTED_TURRET_PLACEABLE_PLACEMENT_RANGE_MAX

	TraceResults viewTraceResults = TraceLine( eyePos, eyePos + player.GetViewVector() * (MOUNTED_TURRET_PLACEABLE_PLACEMENT_RANGE_MAX * 2), [player, turretModel], TRACE_MASK_SOLID, TRACE_COLLISION_GROUP_NONE )
	if ( viewTraceResults.fraction < 1.0 )
	{
		
		float slope = fabs( viewTraceResults.surfaceNormal.x ) + fabs( viewTraceResults.surfaceNormal.y )
		if ( slope < 0.707 )
			maxRange = min( Distance2D( eyePos, viewTraceResults.endPos ), MOUNTED_TURRET_PLACEABLE_PLACEMENT_RANGE_MAX )
	}

	vector idealPos = player.GetOrigin() + (viewVec * MOUNTED_TURRET_PLACEABLE_PLACEMENT_RANGE_MAX)

	MountedTurretPlaceablePlacementInfo placementInfo

	vector fwdStart = eyePos + viewVec * min( MOUNTED_TURRET_PLACEABLE_PLACEMENT_RANGE_MIN, maxRange )
	TraceResults fwdResults = TraceHull( fwdStart, eyePos + viewVec * maxRange, MOUNTED_TURRET_PLACEABLE_BOUND_MINS, <30,30,1>, [player, turretModel], TRACE_MASK_SOLID, TRACE_COLLISION_GROUP_NONE )

	if ( MOUNTED_TURRET_PLACEABLE_DEBUG_DRAW_PLACEMENT )
	{
		DebugDrawLine( fwdStart, fwdResults.endPos, COLOR_RED, true, 0.05 )
		DebugDrawLine( fwdStart, fwdResults.endPos, COLOR_RED, true, 0.05 )
		DebugDrawSphere( fwdResults.endPos, 16, COLOR_RED, true, 0.05 )
		DebugDrawLine( fwdResults.endPos, fwdResults.endPos - MOUNTED_TURRET_PLACEABLE_PLACEMENT_TRACE_OFFSET, COLOR_RED, true, 0.05 )
	}


	TraceResults downResults = TraceHull( fwdResults.endPos, fwdResults.endPos - MOUNTED_TURRET_PLACEABLE_PLACEMENT_TRACE_OFFSET, MOUNTED_TURRET_PLACEABLE_BOUND_MINS, MOUNTED_TURRET_PLACEABLE_BOMB_BOUND_MAXS, [player, turretModel], TRACE_MASK_SOLID, TRACE_COLLISION_GROUP_NONE )

	bool isScriptedPlaceable = false
	if ( IsValid( downResults.hitEnt ) )
	{
		var hitEntClassname = downResults.hitEnt.GetNetworkedClassName()

		if ( hitEntClassname == "func_brush" || hitEntClassname == "script_mover" )
		{
			isScriptedPlaceable = true
		}
	}

	bool success = !downResults.startSolid && downResults.fraction < 1.0 && ( downResults.hitEnt.IsWorld() || downResults.hitEnt.GetNetworkedClassName() == "func_brush" || isScriptedPlaceable )

	entity parentTo
	if ( IsValid( downResults.hitEnt ) && ( downResults.hitEnt.GetNetworkedClassName() == "func_brush" || downResults.hitEnt.GetNetworkedClassName() == "script_mover" ) )
	{
		parentTo = downResults.hitEnt
	}

	if ( downResults.startSolid && downResults.fraction < 1.0 && ( downResults.hitEnt.IsWorld() || isScriptedPlaceable ) )
	{
		TraceResults upResults = TraceHull( downResults.endPos, downResults.endPos, MOUNTED_TURRET_PLACEABLE_BOUND_MINS, MOUNTED_TURRET_PLACEABLE_BOMB_BOUND_MAXS, [player, turretModel], TRACE_MASK_SOLID, TRACE_COLLISION_GROUP_NONE )
		if ( !upResults.startSolid )
			success = true
	}

	
	vector surfaceAngles = viewVec
	if ( downResults.fraction < 1.0 )
	{
		surfaceAngles 	= AnglesOnSurface( downResults.surfaceNormal, viewVec )
		vector newUpDir = AnglesToUp( surfaceAngles )
		vector oldUpDir = AnglesToUp( angles )

		if ( DotProduct( newUpDir, oldUpDir ) < MOUNTED_TURRET_PLACEABLE_ANGLE_LIMIT )
		{
			surfaceAngles = viewVec
			success = false
		}
	}

	if ( success )
	{
		turretModel.SetOrigin( downResults.endPos )
		turretModel.SetAngles( surfaceAngles )
	}

	if ( !player.IsOnGround() )
		success = false

	
	if ( success && downResults.fraction < 1.0 )
	{
		vector right = turretModel.GetRightVector()
		vector forward = turretModel.GetForwardVector()
		vector up = turretModel.GetUpVector()

		float length = Length( MOUNTED_TURRET_PLACEABLE_BOUND_MINS )

		array< vector > groundTestOffsets = [
			( <0,0,0> ),
			( right * 20 ) + ( forward * 12 ),
			( -right * 20 ) + ( forward * 12 ),
			( -forward * 28 )
		]

		foreach ( vector testOffset in groundTestOffsets )
		{
			vector testPos = turretModel.GetOrigin() + testOffset
			TraceResults traceResult = TraceLine( testPos + ( up * MOUNTED_TURRET_PLACEABLE_PLACEMENT_MAX_HEIGHT_DELTA ), testPos + ( up * -MOUNTED_TURRET_PLACEABLE_PLACEMENT_MAX_HEIGHT_DELTA ), [player, turretModel], TRACE_MASK_SOLID | TRACE_MASK_PLAYERSOLID, TRACE_COLLISION_GROUP_NONE )

			if ( MOUNTED_TURRET_PLACEABLE_DEBUG_DRAW_PLACEMENT )
			{
				DebugDrawLine( testPos, traceResult.endPos, COLOR_RED, true, 0.05 )
			}

			if ( traceResult.fraction == 1.0 )
			{
				success = false
				break
			}
		}
	}

	

	

	
	
	if ( success && downResults.hitEnt != null && ( !downResults.hitEnt.IsWorld() && !isScriptedPlaceable ) )
		success = false

	
	if ( success && !PlayerCanSeePos( player, downResults.endPos, true, 90 ) ) 
		success = false

	vector org = success ? downResults.endPos - <0,0,MOUNTED_TURRET_PLACEABLE_BOMB_BOUND_MAXS.x> : idealPos 
	vector ang = success ? surfaceAngles : angles
	placementInfo.success = success
	placementInfo.origin = org
	placementInfo.angles = ang
	placementInfo.parentTo = parentTo

	return placementInfo
}
#endif


void function MinimapPackage_RampartGun( entity ent, var rui )
{
#if MINIMAP_DEBUG
		printt( "Adding 'rui/hud/ultimate_icons/ultimate_rampart' icon to minimap" )
#endif
	RuiSetImage( rui, "defaultIcon", $"rui/hud/ultimate_icons/ultimate_rampart" )
	RuiSetImage( rui, "clampedDefaultIcon", $"rui/hud/ultimate_icons/ultimate_rampart" )
	RuiSetBool( rui, "useTeamColor", false )
	RuiSetFloat( rui, "iconBlend", 0.0 )
}

