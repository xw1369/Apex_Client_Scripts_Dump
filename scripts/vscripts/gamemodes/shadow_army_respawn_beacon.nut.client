
































global function ShadowArmy_RespawnBeacon_Init
global function ShadowArmy_RespawnBeacon_RegisterNetworking
global function ShadowArmy_RespawnBeacon_GetLegendsWaitingToRespawnCount
global function ShadowArmy_RespawnBeacon_CanBeaconBeUsed
global function ShadowArmy_RespawnBeacon_CanBeaconBeUsedByPlayer







global function ShadowArmy_RespawnBeacon_ServerCallback_RespawnBeaconOnUse
global function ShadowArmy_RespawnBeacon_ServerCallback_ShowBeaconHint
global function ShadowArmy_RespawnBeacon_UpdateBeaconMapFeature
global function ShadowArmy_RespawnBeacon_ServerCallback_ManageHoloFX
global function ShadowArmy_RespawnBeacon_ServerCallback_OnBeaconStateChanged



































const string SHADOWARMY_RESPAWNBEACON_USE_HOLD_SFX= "Survival_RespawnBeacon_Linking_loop" 



const float BEACON_INTERACT_TIME = 12.0
const float SUPPORT_LEGEND_BEACON_INTERACT_TIME = 8.0
const float RESPAWN_BEACON_ICON_FADE_DIST_NEAR = 300.0
const float UNSET_TIME = -1


const string WAYPOINT_SHADOWARMY_RESPAWNBEACON = "waypoint_shadowarmy_rspwnbeacon"
const int WAYPOINT_ENT_IDX_RESPAWN_BEACON_ENT = 0
const int WAYPOINT_INT_IDX_BEACON_STATE = 3
const int WAYPOINT_FLOAT_IDX_RESPAWN_BEACON_COOLDOWN_END_TIME = 0
const int WAYPOINT_FLOAT_IDX_RESPAWN_BEACON_COOLDOWN_DURATION = 1
const int WAYPOINT_FLOAT_IDX_RESPAWN_BEACON_USE_END_TIME = 2
const int WAYPOINT_FLOAT_IDX_RESPAWN_BEACON_USE_DURATION = 3

global enum eShadowArmyRespawnBeaconState
{
	IDLE, 
	IN_USE, 
	COOLDOWN, 
	DISABLED_RING, 
	TEMP_DISABLED, 
	INVALID,
	_count
}

enum eShadowArmyRespawnBeaconHintIndex
{
	RESPAWN_BEACON_SPAWN_LEGENDS_HINT,
	RESPAWN_BEACON_INUSE,
	_count
}

struct ShadowArmyRespawnBeaconData
{
	entity beaconEnt
	entity beaconWaypointEnt
}



enum eShadowArmyRespawnBeaconDurationType 
{
	TIME_REMAINING_ON_COOLDOWN,
	FULL_COOLDOWN_DURATION,
	TIME_REMAINING_ON_USE,
	FULL_USE_DURATION,
	_count
}



struct
{
	table < entity, ShadowArmyRespawnBeaconData > beaconToBeaconDataTable


		table<entity, var> minimapIconRuiTable
		table<entity, var> fullmapIconRuiTable








}
file



void function ShadowArmy_RespawnBeacon_Init()
{
	if ( !IsShadowArmyGamemode() )
		return



















		AddCreateCallback( "prop_dynamic", RespawnEntitySpawned )
		AddCreateCallback( "prop_script", RespawnEntitySpawned )
		RegisterMinimapPackage( "prop_script", eMinimapObject_prop_script.RESPAWN_CHAMBER, MINIMAP_OBJECT_RUI, MinimapPackage_ShadowArmyRespawnBeacon, FULLMAP_OBJECT_RUI, FullmapPackage_ShadowArmyRespawnBeacon )
		Waypoints_RegisterCustomType( WAYPOINT_SHADOWARMY_RESPAWNBEACON, OnBeaconWaypointInstanced )
		AddCallback_EntitiesDidLoad( EntitiesDidLoad_Client )
		RegisterSignal( "StopHologramFX" )
		RegisterSignal( "BeaconStateChanged" )





}


void function ShadowArmy_RespawnBeacon_RegisterNetworking()
{
	if ( !IsShadowArmyGamemode() )
		return

	
	Remote_RegisterClientFunction( "ShadowArmy_RespawnBeacon_ServerCallback_RespawnBeaconOnUse", "entity", "entity" )
	Remote_RegisterClientFunction( "ShadowArmy_RespawnBeacon_ServerCallback_ShowBeaconHint", "int", 0, eShadowArmyRespawnBeaconHintIndex._count )
	Remote_RegisterClientFunction( "ShadowArmy_RespawnBeacon_ServerCallback_ManageHoloFX", "entity", "bool" )
	Remote_RegisterClientFunction( "ShadowArmy_RespawnBeacon_ServerCallback_OnBeaconStateChanged", "entity" )
}





































































void function RespawnEntitySpawned( entity ent )
{
	if ( !IsRespawnBeaconEnt( ent ) )
		return















	AddBeaconEntToRespawnBeaconsArray( ent )
	SetCallback_CanUseEntityCallback( ent, CanPlayerUseBeacons )
	SetCallback_ShouldUseBlockReloadCallback( ent, SimpleShouldNotBlockReloadCallback )

	AddCallback_OnUseEntity_ClientServer( ent, RespawnBeaconOnUse )
}










void function EntitiesDidLoad_Client()
{
	ShadowArmy_RespawnBeacon_UpdateBeaconMapFeature()
}


































































void function MinimapPackage_ShadowArmyRespawnBeacon( entity ent, var rui )
{
	RuiSetImage( rui, "defaultIcon", RESPAWN_BEACON_ICON )
	RuiSetImage( rui, "clampedDefaultIcon", $"" )
	RuiSetBool( rui, "useTeamColor", false )

	file.minimapIconRuiTable[ent] <- rui
}
void function FullmapPackage_ShadowArmyRespawnBeacon( entity ent, var rui )
{
	RuiSetImage( rui, "defaultIcon", RESPAWN_BEACON_ICON )
	RuiSetImage( rui, "clampedDefaultIcon", $"" )
	RuiSetBool( rui, "useTeamColor", false )

	file.fullmapIconRuiTable[ent] <- rui
}



void function OnBeaconWaypointInstanced( entity wp )
{
	if ( !IsValid( wp ) )
		return

	if ( wp.GetWaypointCustomType() != WAYPOINT_SHADOWARMY_RESPAWNBEACON )
		return

	
	ShadowArmyRespawnBeaconData beaconData
	entity respawnBeacon = wp.GetWaypointEntity( WAYPOINT_ENT_IDX_RESPAWN_BEACON_ENT )
	beaconData.beaconEnt = respawnBeacon
	beaconData.beaconWaypointEnt = wp
	file.beaconToBeaconDataTable[ respawnBeacon ] <- beaconData
	AddEntityCallback_GetUseEntOverrideText( respawnBeacon, RespawnBeacon_TextOverride )
	var rui = AddOverheadIcon( respawnBeacon, RESPAWN_BEACON_ICON, false, $"ui/overhead_icon_respawn_beacon_states.rpak" )
	RuiSetFloat2( rui, "iconSize", <80, 80, 0> )
	RuiSetFloat( rui, "distanceFade", RESPAWN_BEACON_ICON_FADE_DIST_NEAR )
	RuiSetBool( rui, "adsFade", true )
	RuiSetString( rui, "hint", "#RESPAWN_ALLCAPS" )

	thread ManageRespawnBeaconData_Thread( respawnBeacon, rui )
}









































bool function CanPlayerUseBeacons( entity player, entity ent, int useFlags )
{
	if ( Bleedout_IsBleedingOut( player ) )
		return false

	if ( player.ContextAction_IsActive() )
		return false

	if ( !SURVIVAL_PlayerAllowedToPickup( player ) )
		return false

	if ( ent.e.isBusy )
		return false


	return true
}




bool function ShadowArmy_RespawnBeacon_CanBeaconBeUsedByPlayer( entity player, entity beacon )
{
	if ( !IsValid( player ) || !IsValid( beacon ) )
		return false

	
	if ( !GetIsBeaconInInteractableStateForPlayer( player, beacon ) )
		return false

	
	if ( !GetAreAnyLegendsWaitingToRespawn() )
		return false

	return true
}




bool function GetIsBeaconInInteractableStateForPlayer( entity player, entity beacon )
{
	if ( !IsValid( player ) || !IsValid( beacon ) )
		return false

	bool isRevPlayer = ShadowArmy_IsPlayerOnShadowArmy( player )

	
	if ( !isRevPlayer && ShadowArmy_RespawnBeacon_CanBeaconBeUsed(  beacon ) )
		return true

	return false
}




bool function ShadowArmy_RespawnBeacon_CanBeaconBeUsed( entity beacon )
{
	if ( !IsValid( beacon ) )
		return false

	int beaconState = GetBeaconState( beacon )
	
	if ( beaconState == eShadowArmyRespawnBeaconState.IDLE )
		return true

	return false
}




bool function GetAreAnyLegendsWaitingToRespawn()
{






	return ShadowArmy_RespawnBeacon_GetLegendsWaitingToRespawnCount() > 0
}




int function ShadowArmy_RespawnBeacon_GetLegendsWaitingToRespawnCount()
{
	return GetArrayOfLegendsWaitingToRespawn().len()
}




array< entity > function GetArrayOfLegendsWaitingToRespawn()
{
	array < entity > legendsWaitingToRespawn = []

	if ( GetGameState() < eGameState.Playing )
		return legendsWaitingToRespawn

	array < entity > legendsArray = AllianceProximity_GetAllPlayersInAlliance( SHADOWARMY_LEGEND_ALLIANCE, false )

	foreach ( legend in legendsArray )
	{
		if ( IsValid( legend ) && !IsAlive( legend ) && legend.GetPlayerNetInt( "respawnStatus" ) != eRespawnStatus.PLAYER_ELIMINATED )
			legendsWaitingToRespawn.append( legend )
	}

	return legendsWaitingToRespawn
}




bool function IsRespawnBeaconEnt( entity ent )
{
	if ( ent.GetTargetName() == RESPAWN_CHAMBER_TARGETNAME )
		return true

	return false
}











































































































































void function UpdateMapIcons( entity beacon, asset icon )
{
	if ( beacon in file.minimapIconRuiTable )
	{
		RuiSetImage( file.minimapIconRuiTable[beacon], "defaultIcon", icon )
	}
	if ( beacon in file.fullmapIconRuiTable )
	{
		RuiSetImage( file.fullmapIconRuiTable[beacon], "defaultIcon", icon )
	}
}


void function ShadowArmy_RespawnBeacon_ServerCallback_OnBeaconStateChanged( entity beacon )
{
	if ( !IsValid( beacon ) )
		return

	beacon.Signal( "BeaconStateChanged" )
}




int function GetBeaconState( entity beacon )
{
	if ( !IsValid( beacon ) )
	{
#if DEV
			Assert( false, "Shadow Army Respawn Beacon: GetBeaconState is going to return eShadowArmyRespawnBeaconState.INVALID due to Invalid beacon" )
#endif
		return eShadowArmyRespawnBeaconState.INVALID
	}

	ShadowArmyRespawnBeaconData beaconData = GetBeaconDataFromBeaconEnt( beacon )
	entity beaconWaypoint = beaconData.beaconWaypointEnt

	if ( !IsValid( beaconWaypoint ) || beaconWaypoint.GetWaypointCustomType() != WAYPOINT_SHADOWARMY_RESPAWNBEACON )
		return eShadowArmyRespawnBeaconState.INVALID

	return beaconWaypoint.GetWaypointInt( WAYPOINT_INT_IDX_BEACON_STATE )
}




ShadowArmyRespawnBeaconData function GetBeaconDataFromBeaconEnt( entity beacon )
{
	ShadowArmyRespawnBeaconData data

	if ( !( beacon in file.beaconToBeaconDataTable ) )
		return data

	return file.beaconToBeaconDataTable[ beacon ]
}




float function GetRemainingCooldownDurationOnBeacon( entity beacon )
{
	return GetBeaconDurationForDurationType( beacon, eShadowArmyRespawnBeaconDurationType.TIME_REMAINING_ON_COOLDOWN )
}





float function GetFullDurationOfCooldownOnBeacon( entity beacon )
{
	return GetBeaconDurationForDurationType( beacon, eShadowArmyRespawnBeaconDurationType.FULL_COOLDOWN_DURATION )
}



float function GetBeaconUseDuration( entity player, entity beacon )
{
	float duration = BEACON_INTERACT_TIME

	
	if ( IsValid( player ) && IsValid( beacon ) )
	{
		
		if ( Perks_GetRoleForPlayer( player ) == eCharacterClassRole.SUPPORT )
			duration = SUPPORT_LEGEND_BEACON_INTERACT_TIME
	}

	return duration
}




float function GetRemainingUseDurationOnBeacon( entity beacon )
{
	return GetBeaconDurationForDurationType( beacon, eShadowArmyRespawnBeaconDurationType.TIME_REMAINING_ON_USE )
}





float function GetFullDurationOfUseInteractOnBeacon( entity beacon )
{
	return GetBeaconDurationForDurationType( beacon, eShadowArmyRespawnBeaconDurationType.FULL_USE_DURATION )
}




float function GetBeaconDurationForDurationType( entity beacon, int durationType )
{
	float duration = UNSET_TIME

	if ( !IsValid( beacon ) )
	{
		Warning( "Shadow Army Respawn Beacon: GetBeaconDurationForDurationType going to return UNSET_TIME because the Beacon was not valid for durationType: " + GetEnumString( "eShadowArmyRespawnBeaconDurationType", durationType ) )
		return duration
	}

	if ( durationType < 0 || durationType >= eShadowArmyRespawnBeaconDurationType._count )
	{
#if DEV
			Warning( "Shadow Army Respawn Beacon: Tried to run GetBeaconDurationForDurationType with an invalid duration type: " + durationType + " valid types are: " )
			for ( int i = 0; i < eShadowArmyRespawnBeaconDurationType._count; i++ )
			{
				printt( GetEnumString( "eShadowArmyRespawnBeaconDurationType", i ) + " is index: " + i )
			}
#endif
		return duration
	}

	ShadowArmyRespawnBeaconData beaconData = GetBeaconDataFromBeaconEnt( beacon )
	entity beaconWaypoint = beaconData.beaconWaypointEnt

	if ( IsValid( beaconWaypoint ) && beaconWaypoint.GetWaypointCustomType() == WAYPOINT_SHADOWARMY_RESPAWNBEACON )
	{
		switch( durationType )
		{
			case( eShadowArmyRespawnBeaconDurationType.TIME_REMAINING_ON_COOLDOWN ):
				duration = max( 0.0, ( beaconWaypoint.GetWaypointFloat( WAYPOINT_FLOAT_IDX_RESPAWN_BEACON_COOLDOWN_END_TIME ) - Time() ) )
				break
			case( eShadowArmyRespawnBeaconDurationType.FULL_COOLDOWN_DURATION ):
				duration = beaconWaypoint.GetWaypointFloat( WAYPOINT_FLOAT_IDX_RESPAWN_BEACON_COOLDOWN_DURATION )
				break
			case( eShadowArmyRespawnBeaconDurationType.TIME_REMAINING_ON_USE ):
				duration = max( 0.0, ( beaconWaypoint.GetWaypointFloat( WAYPOINT_FLOAT_IDX_RESPAWN_BEACON_USE_END_TIME ) - Time() ) )
				break
			case( eShadowArmyRespawnBeaconDurationType.FULL_USE_DURATION ):
				duration = beaconWaypoint.GetWaypointFloat( WAYPOINT_FLOAT_IDX_RESPAWN_BEACON_USE_DURATION )
				break
			default:
#if DEV
					Warning( "Shadow Army Respawn Beacon: GetBeaconDurationForDurationType encountered an unsupported duration type in the switch statement: " + durationType )
#endif
				break
		}
	}
#if DEV
	else
	{
		Warning( "Shadow Army Respawn Beacon: GetBeaconDurationForDurationType going to return UNSET_TIME because the Beacon Waypoint was not valid for durationType: " + GetEnumString( "eShadowArmyRespawnBeaconDurationType", durationType ) )
	}
#endif

	return duration
}















void function RespawnBeaconOnUse( entity pickup, entity player, int pickupFlags )
{
	if ( !IsBitFlagSet( pickupFlags, USE_INPUT_LONG ) )
		return

	float time = Time()
	ShadowArmy_RespawnBeaconOnUse_Common( pickup, player, time )









}



void function ShadowArmy_RespawnBeacon_ServerCallback_RespawnBeaconOnUse( entity pickup, entity player )
{
	if ( !IsValid( pickup ) || !IsValid( player ) ) 
		return

	
	
	
	if ( player.p.isInExtendedUse )
	{
		
		return
	}
	else
	{
		ShadowArmy_RespawnBeaconOnUse_Common( pickup, player, Time() )
	}
}



void function ShadowArmy_RespawnBeaconOnUse_Common( entity beacon, entity player, float startTime )
{
	if ( !IsValid( player ) || !IsValid( beacon ) )
		return

	
	
	if ( !ShadowArmy_RespawnBeacon_CanBeaconBeUsedByPlayer( player, beacon ) )
	{




		return
	}

	ExtendedUseSettings settings


		HidePlayerHint( "#RESPAWN_AT_BEACONS_HINT" )
		settings.loopSound = SHADOWARMY_RESPAWNBEACON_USE_HOLD_SFX
		settings.displayRui = $"ui/health_use_progress.rpak"
		settings.displayRuiFunc = DisplayRuiForRespawnBeacon
		settings.icon = $""
		settings.hint = GetUseInProgressHint( beacon )
		settings.icon = RESPAWN_BEACON_ICON
		settings.serverStartTime = startTime










	settings.duration = GetBeaconUseDuration( player, beacon )
	settings.useInputFlag = IN_USE_LONG

	thread ExtendedUse( beacon, player, settings )
}





























































































































































































































































































































































const float POST_SIGNAL_DELAY = 0.1 
void function ManageRespawnBeaconData_Thread( entity beacon, var rui )
{
#if DEV
		Assert( IsNewThread(), "Must be threaded off" )
#endif

	WaitEndFrame() 

	if ( !IsValid( beacon ) )
		return

	if ( !IsValid( rui ) )
		return

	beacon.EndSignal( "OnDestroy" )
	UpdateRespawnChamberRuis( rui, GetAreAnyLegendsWaitingToRespawn() ) 

	
	while ( GetGameState() <= eGameState.Playing )
	{
		int currentBeaconState = GetBeaconState( beacon )
		printt("Shadow Army Respawn Beacon: ManageRespawnBeaconData_Thread is updating after beacon changed state to: " + GetEnumString( "eShadowArmyRespawnBeaconState", currentBeaconState ) )
		switch( currentBeaconState )
		{
			case eShadowArmyRespawnBeaconState.INVALID:
				
				UpdateMapIcons( beacon, $"" )
				break
			case eShadowArmyRespawnBeaconState.IDLE:
				RuiSetFloat3( rui, "iconColor", <0.67, 0.96, 0.32> )
				RuiSetFloat3( rui, "bgColor", <0, 0, 0> )
				RuiSetFloat3( rui, "borderColor", <1, 1, 1> )
				RuiSetBool( rui, "isOnCooldown", false )
				RuiSetBool( rui, "isInUse", false )
				RuiSetBool( rui, "showHint", true )
				
				UpdateMapIcons( beacon, RESPAWN_BEACON_ICON )
				RuiSetFloat( rui, "distanceFade", RESPAWN_BEACON_ICON_FADE_DIST_NEAR )
				break
			case eShadowArmyRespawnBeaconState.IN_USE:
				RuiSetFloat3( rui, "iconColor", <0.67, 0.96, 0.32> )
				RuiSetFloat3( rui, "bgColor", <0, 0, 0> )
				RuiSetFloat3( rui, "borderColor", <1, 1, 1> )
				RuiSetBool( rui, "isOnCooldown", false )
				RuiSetBool( rui, "isInUse", true )
				RuiSetFloat( rui, "remainingInUseTime", GetRemainingUseDurationOnBeacon( beacon ) )
				RuiSetFloat( rui, "maxInUseTime", max( GetFullDurationOfUseInteractOnBeacon( beacon ), 0.01) )
				RuiSetBool( rui, "showHint", false )
				
				var gameRUI = ClGameState_GetRui()
				if ( gameRUI != null )
				{
					RuiSetGameTime( gameRUI, "respawnStartTime", Time() )
					RuiSetGameTime( gameRUI, "respawnEndTime", Time() + GetRemainingUseDurationOnBeacon( beacon ) )
				}
				
				UpdateMapIcons( beacon, RESPAWN_BEACON_INUSE_ICON )
				RuiSetFloat( rui, "distanceFade", 50000 )
				break
			case eShadowArmyRespawnBeaconState.COOLDOWN:
				RuiSetFloat3( rui, "iconColor", <1, 1, 1> )
				RuiSetFloat3( rui, "bgColor", <0, 0, 0> )
				RuiSetFloat3( rui, "borderColor", <1, 1, 1> )
				RuiSetFloat3( rui, "cdStripes" , <0.4, 0.4, 0.4> )
				RuiSetBool( rui, "isOnCooldown", true )
				RuiSetFloat( rui, "remainingCdTime", GetRemainingCooldownDurationOnBeacon( beacon ) )
				RuiSetFloat( rui, "maxCdTime", max(GetFullDurationOfCooldownOnBeacon( beacon ) , 0.01) )
				RuiSetBool( rui, "isInUse", false )
				RuiSetBool( rui, "showHint", false )
				
				var gameRUI = ClGameState_GetRui()
				if ( gameRUI != null )
				{
					RuiSetGameTime( gameRUI, "respawnStartTime", RUI_BADGAMETIME )
					RuiSetGameTime( gameRUI, "respawnEndTime", RUI_BADGAMETIME )
				}
				
				UpdateMapIcons( beacon, RESPAWN_BEACON_DISABLED_ICON )
				RuiSetFloat( rui, "distanceFade", RESPAWN_BEACON_ICON_FADE_DIST_NEAR )
				break
			case eShadowArmyRespawnBeaconState.DISABLED_RING:
				RuiSetBool( rui, "isRingDisabled", true )
				RuiSetBool( rui, "isOnCooldown", false )
				RuiSetBool( rui, "isInUse", false )
				RuiSetBool( rui, "showHint", false )
				
				UpdateMapIcons( beacon, $"" )
				RuiSetFloat( rui, "distanceFade", RESPAWN_BEACON_ICON_FADE_DIST_NEAR )
				break
			case eShadowArmyRespawnBeaconState.TEMP_DISABLED:
				RuiSetFloat3( rui, "iconColor", <0.2, 0.2, 0.2> )
				RuiSetFloat3( rui, "bgColor", <0.5, 0.5, 0.5> )
				RuiSetFloat3( rui, "borderColor", <0, 0, 0> )
				RuiSetBool( rui, "isOnCooldown", false )
				RuiSetBool( rui, "isInUse", false )
				RuiSetBool( rui, "showHint", false )
				
				UpdateMapIcons( beacon, RESPAWN_BEACON_DISABLED_ICON )
				RuiSetFloat( rui, "distanceFade", RESPAWN_BEACON_ICON_FADE_DIST_NEAR )
				break
			default:
				
				UpdateMapIcons( beacon, $"" )
#if DEV
					Assert( false, "Shadow Army Respawn Beacon: Unsupported beacon state: " + currentBeaconState + " for beacon: " + beacon + " in ManageRespawnBeaconData_Thread" )
#endif
				break
		}

		beacon.WaitSignal( "BeaconStateChanged" )
		wait POST_SIGNAL_DELAY
	}
}
































void function ShadowArmy_RespawnBeacon_ServerCallback_ManageHoloFX( entity beacon, bool isStartingFX )
{
	if ( !IsValid( beacon ) )
		return

	if ( isStartingFX )
		PlayBeaconHologramVFX( beacon )
	else
		StopBeaconHologramVFX( beacon )
}




void function PlayBeaconHologramVFX( entity beacon )
{
	int currentBeaconState = GetBeaconState( beacon )
	if ( currentBeaconState == eShadowArmyRespawnBeaconState.IDLE || currentBeaconState == eShadowArmyRespawnBeaconState.IN_USE )
		thread ManageRespawnBeaconHologramVFX_Thread( beacon )
}




void function StopBeaconHologramVFX( entity beacon )
{
	beacon.Signal( "StopHologramFX" )
}



const float VFX_OFFSET = 100.0
const float RESPAWN_BEACON_HOLO_EFFECT_HEIGHT = 75.0

void function ManageRespawnBeaconHologramVFX_Thread( entity beacon )
{
#if DEV
		Assert( IsNewThread(), "Must be threaded off" )
#endif

	if ( !IsValid( beacon ) )
		return

	beacon.Signal( "StopHologramFX" ) 
	EndSignal( beacon, "OnDestroy", "StopHologramFX" )

	vector beaconAngles = beacon.GetAngles()
	vector fwd    = AnglesToForward( beaconAngles )
	vector up     = AnglesToUp( beaconAngles )
	vector rgt    = AnglesToRight( beaconAngles )
	vector offset = up * VFX_OFFSET
	vector angles = AnglesCompose( beaconAngles, <0, 0, -10> )

	entity fxHolder = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", beacon.GetOrigin() + up * RESPAWN_BEACON_HOLO_EFFECT_HEIGHT, <-90, 0, 0> )
	array<int> fx
	fx.append( StartParticleEffectOnEntity( fxHolder, GetParticleSystemIndex( RESPAWN_BEACON_EMITTER_FX ), FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID ) )

	OnThreadEnd(
		function() : ( fx, fxHolder )
		{
			foreach ( effect in fx )
			{
				EffectStop( effect, false, true )
			}
			fxHolder.Destroy()
		}
	)

	WaitForever()
}




string function RespawnBeacon_TextOverride( entity beacon )
{
	entity localPlayer = GetLocalClientPlayer()
	string hintString = ""

	if ( !IsValid( localPlayer ) || !IsValid( beacon ) )
		return hintString

	bool isRevPlayer = ShadowArmy_IsPlayerOnShadowArmy( localPlayer )
	int currentBeaconState = GetBeaconState( beacon )

	switch( currentBeaconState )
	{
		case eShadowArmyRespawnBeaconState.INVALID:
			break
		case eShadowArmyRespawnBeaconState.IDLE:
			if ( isRevPlayer )
				hintString = "#SHADOW_ARMY_RESPAWNBEACON_REV_USE"
			else if ( !GetAreAnyLegendsWaitingToRespawn() )
				hintString = "#SHADOW_ARMY_RESPAWNBEACON_NO_RESPAWNS"
			else
				hintString = "#SHADOW_ARMY_RESPAWNBEACON_IDLE_USE"
			break
		case eShadowArmyRespawnBeaconState.IN_USE:
			break
		case eShadowArmyRespawnBeaconState.COOLDOWN:
			hintString = Localize( "#SHADOW_ARMY_RESPAWNBEACON_COOLDOWN", int( GetRemainingCooldownDurationOnBeacon( beacon ) ) )
			break
		case eShadowArmyRespawnBeaconState.DISABLED_RING:
			hintString = "#SHADOW_ARMY_RESPAWNBEACON_DISABLED"
			break
		case eShadowArmyRespawnBeaconState.TEMP_DISABLED:
			hintString = "#SHADOW_ARMY_RESPAWNBEACON_DISABLED_TEMP"
			break
		default:
#if DEV
				Assert( false, "Shadow Army Respawn Beacon: Unsupported beacon state: " + currentBeaconState + " for beacon: " + beacon + " in RespawnBeacon_TextOverride" )
#endif
			break
	}

	return hintString
}





string function GetUseInProgressHint( entity beacon )
{
	entity localPlayer = GetLocalClientPlayer()
	string hintString = ""

	if ( !IsValid( localPlayer ) || !IsValid( beacon ) )
		return hintString

	if ( ShadowArmy_IsPlayerOnShadowArmy( localPlayer ) )
		return hintString

	
	if ( GetBeaconState( beacon ) == eShadowArmyRespawnBeaconState.IN_USE || GetBeaconState( beacon ) == eShadowArmyRespawnBeaconState.IDLE )
	{
		
		if ( Perks_GetRoleForPlayer( localPlayer ) == eCharacterClassRole.SUPPORT )
			hintString = "#SHADOW_ARMY_RESPAWNBEACON_USING_SUPPORT"
		else
			hintString = "#SHADOW_ARMY_RESPAWNBEACON_USING"
	}

	return hintString
}



void function DisplayRuiForRespawnBeacon( entity ent, entity player, var rui, ExtendedUseSettings settings )
{
	float startTime = settings.serverStartTime > 0.0 ? settings.serverStartTime : Time()
	float endTime = startTime + settings.duration
	DisplayRuiForRespawnBeacon_Internal( rui, settings.icon, startTime, endTime, settings.hint )
}



void function DisplayRuiForRespawnBeacon_Internal( var rui, asset icon, float startTime, float endTime, string hint )
{
	RuiSetBool( rui, "isVisible", true )
	RuiSetImage( rui, "icon", icon )
	RuiSetGameTime( rui, "startTime", startTime )
	RuiSetGameTime( rui, "endTime", endTime )
	RuiSetString( rui, "hintKeyboardMouse", hint )
	RuiSetString( rui, "hintController", hint )
}









































































const float HINT_DISPLAY_TIME = 5.0
const float HINT_FADE_TIME = 1.0
void function ShadowArmy_RespawnBeacon_ServerCallback_ShowBeaconHint( int hintIndex )
{
	string hintText = ""

	switch( hintIndex )
	{
		case eShadowArmyRespawnBeaconHintIndex.RESPAWN_BEACON_SPAWN_LEGENDS_HINT:
			hintText = Localize( "#SHADOW_ARMY_RESPAWNBEACON_SPAWN_HINT", ShadowArmy_RespawnBeacon_GetLegendsWaitingToRespawnCount() )
			break
		case eShadowArmyRespawnBeaconHintIndex.RESPAWN_BEACON_INUSE:
			hintText = "#SHADOW_ARMY_RESPAWNBEACON_USE_HINT"
			break
		default:
#if DEV
				Assert( false, "Shadow Army Respawn Beacon: Unhandled hintIndex: " + hintIndex )
#endif
			break
	}

	AddPlayerHint( HINT_DISPLAY_TIME, HINT_FADE_TIME, $"", hintText )
}




const int MAP_FEATURE_PRIORITY = 1100
void function ShadowArmy_RespawnBeacon_UpdateBeaconMapFeature()
{
	int numLegendsWaitingToRespawn = ShadowArmy_RespawnBeacon_GetLegendsWaitingToRespawnCount()
	RemoveMapFeatureItemByName( "#RESPAWN_BEACON" )

	if ( numLegendsWaitingToRespawn > 0 )
		SetMapFeatureItem( MAP_FEATURE_PRIORITY, "#RESPAWN_BEACON", Localize( "#SHADOW_ARMY_RESPAWN_BEACON_DESC_AVAIL", string( numLegendsWaitingToRespawn ) ), RESPAWN_BEACON_ICON )
	else
		SetMapFeatureItem( MAP_FEATURE_PRIORITY, "#RESPAWN_BEACON", "#SHADOW_ARMY_RESPAWN_BEACON_DESC_NOT_AVAIL", RESPAWN_BEACON_ICON )
}






























































































































































































































































































                                  