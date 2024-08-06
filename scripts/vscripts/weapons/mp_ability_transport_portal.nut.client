
global function OnWeaponActivate_ability_transport_portal
global function OnWeaponDeactivate_ability_transport_portal
global function OnWeaponTossPrep_ability_transport_portal
global function OnWeaponTossReleaseAnimEvent_ability_transport_portal
global function MpAbilityTransportPortal_Init

global function TransportPortal_IsPlayerCurrentlyChanneling
global function TransportPortal_GetReceiverPlayerIsLookingAt









global function ServerToClient_TransportPortal_UltDurationChanged
global function IsPlayersBeingWarnedAboutNexus
global function TransportPortal_SetChannelUseTime


const float TRANSPORT_PORTAL_ROTATE_SPEED_SLOW = 360.0 * 0.1
const float TRANSPORT_PORTAL_ROTATE_SPEED_FAST = 360.0 * 1.75
const float TRANSPORT_PORTAL_ROTATE_SPEED_ENDING = 360.0 * 0.75
const int TRANSPORT_PORTAL_TRIGGER_RADIUS = 20

const vector CHASE_PORTAL_OFFSET = <0,0,50>

global const float TRANSPORT_PORTAL_TRANSLOCATOR_PORTAL_OFFSET = 90.0

const vector ALTER_PURPLE_COLOR = <147,112,219>







const string TRANSPORT_PORTAL_HINT_THREAD = "alter_ult_hint_thread"
const string TRANSPORT_PORTAL_SPECTATOR_CHANGED = "alter_ult_spectator_changed"
const string TRANSPORT_PORTAL_PREVIEW_END = "alter_ult_preview_end"
const string TRANSPORT_PORTAL_LONG_HOLD_END = "alter_ult_long_hold_end"



global const string TRANSPORT_PORTAL_ROOT_SCRIPTNAME = "alter_ult_root"
global const string TRANSPORT_PORTAL_RECEIVER_SCRIPTNAME = "alter_ult_receiver"
const string TRANSPORT_PORTAL_TRANSLOCATOR_SCRIPTNAME = "alter_ult_translocator"
global const string TRANSPORT_PORTAL_TRANSLOCATOR_TRACE_BLOCKER_SCRIPTNAME = "alter_ult_translocator_trace_blocker"
const string TRANSPORT_PORTAL_WARNING_TRIGGER_SCRIPTNAME = "alter_ult_warning_trigger"
global const string TRANSPORT_PORTAL_ALLY_PORTAL_SCRIPTNAME = "alter_ult_ally_portal"
global const string TRANSPORT_PORTAL_ALLY_PORTAL_TRACE_BLOCKER_SCRIPTNAME = "alter_ult_ally_trace_blocker"
global const string TRANSPORT_PORTAL_WEAPON_NAME = "mp_ability_transport_portal"


const string TRANSPORT_PORTAL_SERVER_TO_CLIENT_ULT_DURATION_CHANGED = "ServerToClient_TransportPortal_UltDurationChanged"

const string TRANSPORT_PORTAL_CLIENT_TO_SERVER_RECALL_ULT = "ClientToServer_TransportPortal_RecallUlt"
const string TRANSPORT_PORTAL_CLIENT_TO_SERVER_TRY_CHANNEL = "ClientToServer_TransportPortal_TryChannel"


const string TRANSPORT_PORTAL_EXPIRE_TIME_NETVAR = "TransportPortal_ExpireTime"




const asset PORTAL_RECIEVER_MODEL = $"mdl/props/alter_teleporter/alter_teleporter.rmdl"
const asset PORTAL_TRANSLOCATOR_MODEL = $"mdl/props/alter_teleporter/alter_teleporter_portal.rmdl"


const asset TRANSPORT_PORTAL_AR_EDGE_FX = $"P_alter_ulti_ar_edge"
const asset TRANSPORT_PORTAL_ENTER_EXIT_FX = $"P_alter_ulti_portal_announce"
const asset TRANSPORT_PORTAL_ONEWAY_START = $"P_alter_ulti_portal_in"
const asset TRANSPORT_PORTAL_WARNING_FRIENDLY = $"P_alter_ulti_portal_warning_friendly"
const asset TRANSPORT_PORTAL_WARNING_ENEMY = $"P_alter_ulti_portal_warning_enemy"
const asset TRANSPORT_PORTAL_RECIEVER_IDLE = $"P_alter_ulti_prop_idle"
const asset TRANSPORT_PORTAL_RECIEVER_ACTIVE = $"P_alter_ulti_prop_activate"
const asset TRANSPORT_PORTAL_TRANSLOCATOR_IDLE = $"P_alter_ulti_prop_idle_top"
const asset TRANSPORT_PORTAL_TRANSLOCATOR_ACTIVE = $"P_alter_ulti_prop_activate_top"
const asset TRANSPORT_PORTAL_CHASE_INACTIVE = $"P_alter_ulti_portal_inactive"
const asset TRANSPORT_PORTAL_CHASE_PRE_ACTIVE = $"P_alter_ulti_portal_inactive_refrac"
const asset TRANSPORT_PORTAL_CHASE_ACTIVE = $"P_alter_ulti_portal_init"
const asset TRANSPORT_PORTAL_DESTROYED = $"P_alter_ulti_portal_prop_exp"


const string TRANSPORT_PORTAL_PLACED_SOUND = "Alter_Ult_Base_Activate_3p"
const string TRANSPORT_PORTAL_END_SOUND = "Alter_Ult_Base_Bottom_Dissolve_3p"
const string TRANSPORT_PORTAL_CLOSE_SOUND = "Alter_Ult_Base_Expire_3p"
const string TRANSPORT_PORTAL_CHASE_PORTAL_WARMUP_SOUND = "Alter_Ult_ChasePortal_Channeling_3p"
const string TRANSPORT_PORTAL_CHASE_PORTAL_ACTIVE_SOUND = "Alter_Ult_ChasePortal_Active_3p"
const string TRANSPORT_PORTAL_CHASE_PORTAL_ENDING_SOUND = "Alter_Ult_ChasePortal_Ending_3p"
const string TRANSPORT_PORTAL_AMBIENT_IDLE_HUM_SOUND = "Alter_Ult_Base_Idle_Inactive_3p"
const string TRANSPORT_PORTAL_AMBIENT_ACTIVE_HUM_SOUND = "Alter_Ult_Base_Idle_Active_3p"
const string TRANSPORT_PORTAL_DESTROYED_SOUND = "Alter_Ult_Base_Destroyed_3p"
const string TRANSPORT_PORTAL_ARRIVAL_WARNING_SOUND_FRIENDLY_3P = "Alter_Ult_Base_Alert_ArrivalWarning_Friendly_3p"
const string TRANSPORT_PORTAL_ARRIVAL_WARNING_SOUND_ENEMY_3P = "Alter_Ult_Base_Alert_ArrivalWarning_Enemy_3p"


global const asset TRANSPORT_PORTAL_IN_WORLD_HUD_OBJECT = $"ui/in_world_alter_recall_icon.rpak"
const asset TRANSPORT_PORTAL_RECEIVER_IMAGE = $"rui/hud/ultimate_icons/ultimate_alter"
const asset TRANSPORT_PORTAL_PORTAL_ICON = $"rui/hud/ping/icon_ping_phase_tunnel"

const bool TRANSPORT_PORTAL_DATAPAD_DEBUG = true
#if DEV
global const bool TRANSPORT_PORTAL_NAVMESH_PATH_DEBUG = false
#endif














struct AllyPortalData
{





}

global enum eTransportPortalDestructionReason
{
	NONE,
	ENEMY,
	WORLDSPAWN,
	TIMEOUT,
	CANCELLED,

	_COUNT
}

struct{






























	float receiverLifespan = 120
	float receiverLifespanUpgradeAmount = 30
	bool receiverLastsForeverWithUpgrade = true

	bool canUseWithCharacterActionWhenKnocked = true
	bool allowRemotePackup = false

	float incomingWarningRadius = 15.0

	float maxUseDistance = 200

	float portalWarmupTime = 1.0

	float useTime = 0.3
}tuning

struct
{
	table<entity, AllyPortalData > allyPortalToDataMap







	table< int, array<entity> > teamToReceiversMap


	float useStartTime = -1
	array< entity > allReceivers
	array< entity > warningTriggers
	entity ultPendingRuiDurationUpdate = null

	float channelStartTime = -1
	float channelTime = 0
	float channelTimeElapsed = 0

}file

void function MpAbilityTransportPortal_Init()
{
	SetupTuning()

	PrecacheScriptString( TRANSPORT_PORTAL_ROOT_SCRIPTNAME )
	PrecacheScriptString( TRANSPORT_PORTAL_RECEIVER_SCRIPTNAME )
	PrecacheScriptString( TRANSPORT_PORTAL_TRANSLOCATOR_SCRIPTNAME )
	PrecacheScriptString( TRANSPORT_PORTAL_TRANSLOCATOR_TRACE_BLOCKER_SCRIPTNAME )
	PrecacheScriptString( TRANSPORT_PORTAL_WARNING_TRIGGER_SCRIPTNAME )
	PrecacheScriptString( TRANSPORT_PORTAL_ALLY_PORTAL_SCRIPTNAME )
	PrecacheScriptString( TRANSPORT_PORTAL_ALLY_PORTAL_TRACE_BLOCKER_SCRIPTNAME )

	PrecacheModel( PORTAL_RECIEVER_MODEL )
	PrecacheModel( PORTAL_TRANSLOCATOR_MODEL )

	PrecacheParticleSystem( TRANSPORT_PORTAL_AR_EDGE_FX )
	PrecacheParticleSystem( TRANSPORT_PORTAL_ENTER_EXIT_FX )
	PrecacheParticleSystem( TRANSPORT_PORTAL_ONEWAY_START )
	PrecacheParticleSystem( TRANSPORT_PORTAL_WARNING_FRIENDLY )
	PrecacheParticleSystem( TRANSPORT_PORTAL_WARNING_ENEMY )
	PrecacheParticleSystem( TRANSPORT_PORTAL_RECIEVER_IDLE )
	PrecacheParticleSystem( TRANSPORT_PORTAL_RECIEVER_ACTIVE )
	PrecacheParticleSystem( TRANSPORT_PORTAL_TRANSLOCATOR_IDLE )
	PrecacheParticleSystem( TRANSPORT_PORTAL_TRANSLOCATOR_ACTIVE )
	PrecacheParticleSystem( TRANSPORT_PORTAL_CHASE_INACTIVE )
	PrecacheParticleSystem( TRANSPORT_PORTAL_CHASE_PRE_ACTIVE )
	PrecacheParticleSystem( TRANSPORT_PORTAL_CHASE_ACTIVE )
	PrecacheParticleSystem( TRANSPORT_PORTAL_DESTROYED )






	RegisterSignal( TRANSPORT_PORTAL_HINT_THREAD )
	RegisterSignal( TRANSPORT_PORTAL_SPECTATOR_CHANGED )
	RegisterSignal( TRANSPORT_PORTAL_PREVIEW_END )
	RegisterSignal( TRANSPORT_PORTAL_LONG_HOLD_END )


	if ( tuning.allowRemotePackup )
	{
		Remote_RegisterServerFunction( TRANSPORT_PORTAL_CLIENT_TO_SERVER_RECALL_ULT )
	}
	else if ( tuning.canUseWithCharacterActionWhenKnocked )
	{
		Remote_RegisterServerFunction( TRANSPORT_PORTAL_CLIENT_TO_SERVER_TRY_CHANNEL )
	}

	Remote_RegisterClientFunction( TRANSPORT_PORTAL_SERVER_TO_CLIENT_ULT_DURATION_CHANGED, "entity" )

	AddGetExtendedRangeUseEntityCallback( GetExtendedRangeUseEntityCallbackForPlayer )

	RegisterNetworkedVariable( TRANSPORT_PORTAL_EXPIRE_TIME_NETVAR, SNDC_PLAYER_EXCLUSIVE, SNVT_TIME, 0.0 )






















	AddCreateCallback( "prop_script", OnPropScriptCreated )
	AddCreateCallback( "trigger_cylinder_networked", OnPropScriptCreated )

	RegisterConCommandTriggeredCallback( "+scriptCommand5", TransportPortal_OnCharacterButtonPressed )

	AddCallback_OnBleedoutStarted( OnBleedoutStarted )
	AddCallback_OnYouRespawned( OnYouRespawned )
	AddOnSpectatorTargetChangedCallback( OnSpectatorTargetChanged )

	RegisterMinimapPackage( "prop_script", eMinimapObject_prop_script.TRANSPORT_PORTAL_TRANSLOCATOR,
		MINIMAP_OBJECT_RUI, SetupMapRuiTranslocator, FULLMAP_OBJECT_RUI, SetupMapRuiTranslocator )

	RegisterMinimapPackage( "prop_script", eMinimapObject_prop_script.TRANSPORT_PORTAL_RECEIVER,
		MINIMAP_OBJ_AREA_RUI, void function( entity ent, var rui ) {
			SetupMapRuiReceiver( ent, rui, false )
		},
		FULLMAP_OBJECTIVE_AREA_RUI, void function( entity ent, var rui ) {
			SetupMapRuiReceiver( ent, rui, true )
		}
	)

	RegisterMinimapPackage( "prop_script", eMinimapObject_prop_script.TRANSPORT_PORTAL_RECEIVER_PREVIEW,
		MINIMAP_OBJ_AREA_RUI, void function( entity ent, var rui ) {
			SetupMapRuiReceiverPreview( ent, rui, false )
		},
		FULLMAP_OBJECTIVE_AREA_RUI, void function( entity ent, var rui ) {
			SetupMapRuiReceiverPreview( ent, rui, true )
		}
	)

	RegisterMinimapPackage( "prop_script", eMinimapObject_prop_script.TRANSPORT_PORTAL_CHASE_PORTAL,
		MINIMAP_OBJECT_RUI, SetupMapRuiChasePortal, FULLMAP_OBJECT_RUI, SetupMapRuiChasePortal )


}

void function SetupTuning()
{




















	tuning.receiverLifespan = 					GetCurrentPlaylistVarFloat	( "alter_ult_receiverLifespan", tuning.receiverLifespan )
	tuning.receiverLifespanUpgradeAmount = 		GetCurrentPlaylistVarFloat	( "alter_ult_receiverLifespanUpgradeAmount", tuning.receiverLifespanUpgradeAmount )
	tuning.receiverLastsForeverWithUpgrade = 	GetCurrentPlaylistVarBool	( "alter_ult_receiverLastsForeverWithUpgrade", tuning.receiverLastsForeverWithUpgrade )

	tuning.canUseWithCharacterActionWhenKnocked = GetCurrentPlaylistVarBool	( "alter_ult_canUseWithCharacterActionWhenKnocked", tuning.canUseWithCharacterActionWhenKnocked )
	tuning.allowRemotePackup 					= GetCurrentPlaylistVarBool	( "alter_ult_allowRemotePackup", tuning.allowRemotePackup )

	Assert( !(tuning.canUseWithCharacterActionWhenKnocked && tuning.allowRemotePackup), "Can't use both remote packup and character action when knocked! They conflict on controls"  )

	tuning.incomingWarningRadius =				GetCurrentPlaylistVarFloat	( "alter_ult_incomingWarningRadius", tuning.incomingWarningRadius ) * METERS_TO_INCHES

	tuning.maxUseDistance = 					GetCurrentPlaylistVarFloat	( "alter_ult_maxUseDist", tuning.maxUseDistance ) * METERS_TO_INCHES

	tuning.portalWarmupTime = 					GetCurrentPlaylistVarFloat	( "alter_ult_portalWarmupTime", tuning.portalWarmupTime )

	tuning.useTime = 							GetCurrentPlaylistVarFloat	( "alter_ult_useTime", tuning.useTime )
}







































entity function GetTransportPortalWeaponFromOwner( entity owner )
{
	entity ultWeapon = owner.GetOffhandWeapon( OFFHAND_ULTIMATE )
	if ( IsValid( ultWeapon ) && ultWeapon.GetWeaponBaseClassName() == TRANSPORT_PORTAL_WEAPON_NAME )
	{
		return ultWeapon
	}
	return null
}

void function OnWeaponActivate_ability_transport_portal( entity weapon )
{
}


void function OnWeaponDeactivate_ability_transport_portal( entity weapon )
{

	weapon.Signal( TRANSPORT_PORTAL_PREVIEW_END )

}

void function OnWeaponTossPrep_ability_transport_portal( entity weapon, WeaponTossPrepParams prepParams )
{

	thread PreviewMinimapLocation_Thread( weapon )

}


void function PreviewMinimapLocation_Thread( entity weapon )
{
	EndSignal( weapon, TRANSPORT_PORTAL_PREVIEW_END, "OnDestroy" )

	wait 0.2

	entity proxy = CreateClientSidePropDynamic( <0, 0, 0>, <0, 90, 0>, $"mdl/dev/empty_model.rmdl" )
	EndSignal( proxy, "OnDestroy" )

	proxy.e.clientEntMinimapClassName = "prop_script"
	proxy.e.clientEntMinimapCustomState = eMinimapObject_prop_script.TRANSPORT_PORTAL_RECEIVER_PREVIEW
	proxy.e.clientEntMinimapFlags = MINIMAP_FLAG_VISIBILITY_SHOW
	proxy.e.clientEntMinimapScale = tuning.maxUseDistance / 16384.0
	proxy.e.clientEntMinimapZOrder = MINIMAP_Z_OBJECT
	thread MinimapObjectThread( proxy )

	int fxId = GetParticleSystemIndex( TRANSPORT_PORTAL_AR_EDGE_FX )
	int pulseVFX  = StartParticleEffectOnEntity( proxy, fxId, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetControlPointVector( pulseVFX, 1, <tuning.maxUseDistance, 0, 0> )

	var overlayRui = CreateCockpitPostFXRui( $"ui/ult_deployment.rpak", HUD_Z_BASE )
	RuiSetVisible( overlayRui, true )

	OnThreadEnd(
		function() : ( proxy, overlayRui )
		{
			if ( IsValid( proxy ) )
				proxy.Destroy()

			RuiDestroyIfAlive( overlayRui )
		}
	)

	while( true )
	{
		vector dropPosition = weapon.GetMostRecentGrenadeImpactPos()
		proxy.SetOrigin( dropPosition )
		WaitFrame()
	}
}


var function OnWeaponTossReleaseAnimEvent_ability_transport_portal( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity player = weapon.GetWeaponOwner()


		weapon.Signal( TRANSPORT_PORTAL_PREVIEW_END )


	weapon.EmitWeaponSound_1p3p( GetGrenadeThrowSound_1p( weapon ), GetGrenadeThrowSound_3p( weapon ) )

	entity deployable = ThrowDeployable( weapon, attackParams, 1.0, OnTransportPortalPlanted, null, <0, 0, 200> )
	if ( deployable )
	{
		PlayerUsedOffhand( player, weapon, true, deployable )













	}

	return weapon.GetAmmoPerShot()
}


void function OnPropScriptCreated( entity ent )
{
	entity player = GetLocalViewPlayer()
	if ( ent.GetScriptName() == TRANSPORT_PORTAL_RECEIVER_SCRIPTNAME )
	{
		file.allReceivers.append( ent )
		if ( ent.GetTeam() == player.GetTeam() )
		{
			thread ManageReceiverLifetime_Thread( ent )
			SetCallback_CanUseEntityCallback( ent, Receiver_CanUseCallback )
			SetCallback_ShouldUseBlockReloadCallback( ent, SimpleShouldNotBlockReloadCallback )
			AddEntityCallback_GetUseEntOverrideText( ent, Receiver_TextOverride )

			AddCallback_OnUseEntity_ClientServer( ent, OnUse_Receiver )

			entity rootEnt = ent.GetOwner()
			if ( IsValid( rootEnt ) )
			{
				thread ManageLookAtRui_Thread( ent, player )

				if ( !rootEnt.GetUseStateByIndex( player.GetPlayerIndex() ) )
				{
					thread TransportPortalCreatedHint_Thread( ent, player, 3 )
				}
			}
		}
	}
	else if ( ent.GetScriptName() == TRANSPORT_PORTAL_ALLY_PORTAL_SCRIPTNAME )
	{
		thread ChasePortalLifetime_Client_Thread( ent )
	}
	else if ( ent.GetScriptName() == TRANSPORT_PORTAL_WARNING_TRIGGER_SCRIPTNAME )
	{
		file.warningTriggers.append( ent )
		thread CleanupWarningTriggerOnDestroy_Thread( ent )
	}
}































































entity function GetReceiverForTryChannel( entity player )
{
	if ( !IsAlive( player ) )
		return null

	if ( !tuning.canUseWithCharacterActionWhenKnocked )
		return null

	if ( !Bleedout_IsBleedingOut( player ) )
		return null

	int team = player.GetTeam()
	if ( !(team in file.teamToReceiversMap) )
		return null

	entity selectedReceiver = null
	foreach ( entity receiver in file.teamToReceiversMap[team] )
	{
		if ( !InRangeOfReceiver( player, receiver ) )
			continue

		if ( !Receiver_CanUseStandardChecks( player, receiver ) )
			continue

		if ( player.p.isInExtendedUse )
			continue

		entity activeWeapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
		if ( IsValid( activeWeapon ) && activeWeapon.GetWeaponClassName() == TRANSPORT_PORTAL_DATAPAD_WEAPON_NAME )
			continue

		selectedReceiver = receiver
		break
	}

	return selectedReceiver
}


void function TransportPortal_OnCharacterButtonPressed( entity player )
{
	if ( tuning.allowRemotePackup )
	{
		Remote_ServerCallFunction( TRANSPORT_PORTAL_CLIENT_TO_SERVER_RECALL_ULT )
	}
	else if ( tuning.canUseWithCharacterActionWhenKnocked )
	{
		if ( IsValid( GetReceiverForTryChannel( player ) ) )
		{
			Remote_ServerCallFunction( TRANSPORT_PORTAL_CLIENT_TO_SERVER_TRY_CHANNEL )
		}
	}
}





























































void function OnTransportPortalPlanted( entity projectile, DeployableCollisionParams collisionParams )
{





















































}

#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________CreateTransportPortal___________________________(){}
#endif

























































































































































































































































































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________ReceiverLifetime___________________________(){}
#endif
void function ManageReceiverLifetime_Thread( entity receiver )
{
	Assert ( IsNewThread(), "Must be started as new thread" )

	int team = receiver.GetTeam()

	if ( !(team in file.teamToReceiversMap) )
	{
		file.teamToReceiversMap[team] <- []
	}
	file.teamToReceiversMap[team].append( receiver )










	receiver.EndSignal( "OnDestroy", "OnBeginDissolve" )

	OnThreadEnd(
		function() : ( receiver, team )
		{













			file.teamToReceiversMap[team].fastremovebyvalue( receiver )
			if ( file.teamToReceiversMap[team].len() == 0 )
			{
				delete file.teamToReceiversMap[team]
			}
		}
	)









































































		WaitForever()

}


























































































































































































































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________ReceiverUse___________________________(){}
#endif

bool function TransportPortal_IsPlayerCurrentlyChanneling( entity player )
{
	entity activeWeapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
	if ( IsValid( activeWeapon ) && activeWeapon.GetWeaponClassName() == TRANSPORT_PORTAL_DATAPAD_WEAPON_NAME )
		return true

	return false
}

void function GetExtendedRangeUseEntityCallbackForPlayer( entity player, array<entity> outEnts )
{
	int team = player.GetTeam()
	if ( team in file.teamToReceiversMap )
	{
		outEnts.extend( file.teamToReceiversMap[team] )
	}
}

bool function Receiver_CanUseCallback( entity player, entity receiver, int useFlags )
{




	if ( !Receiver_CanUseStandardChecks( player, receiver ) )
		return false

	if ( player.p.isInExtendedUse )
		return false

	entity activeWeapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
	if ( IsValid( activeWeapon ) && activeWeapon.GetWeaponClassName() == TRANSPORT_PORTAL_DATAPAD_WEAPON_NAME )
		return false








	return true
}

bool function Receiver_CanUseStandardChecks( entity player, entity receiver )
{
	if ( !IsValid ( player ) || !IsValid( receiver ) )
		return false

	if ( receiver.GetTeam() != player.GetTeam() )
		return false

	if ( StatusEffect_HasSeverity( player, eStatusEffect.placing_phase_tunnel ) )
		return false

	if ( player.Player_IsSkywardLaunching() )
		return false

	if ( player.Player_IsSkywardFollowing() )
		return false

	if ( player.Player_IsSkydiving() )
		return false

	if ( player.ContextAction_IsActive() )
		return false

	if ( player.ContextAction_IsBusy() )
		return false

	if ( player.IsPlayerInAnyVehicle() )
		return false

	if ( receiver.IsDissolving() )
		return false

	entity rootEnt = receiver.GetOwner()
	if ( !IsValid( rootEnt ) )
		return false

	if ( IsBitFlagSet( player.GetWeaponDisableFlags(), WEAPON_DISABLE_FLAGS_MAIN) )
		return false

	if ( rootEnt.GetUseStateByIndex( player.GetPlayerIndex() ) )
		return false

	entity weapon = player.GetOffhandWeapon( OFFHAND_EQUIPMENT )
	if ( IsValid( weapon ) )
	{
		if ( weapon.GetWeaponClassName() == TRANSPORT_PORTAL_DATAPAD_WEAPON_NAME )
		{
			return false
		}
	}

	return true
}

void function OnUse_Receiver( entity receiver, entity player, int useInputFlags )
{
	if ( !IsBitFlagSet( useInputFlags, USE_INPUT_LONG ) )
		return

	if ( IsBitFlagSet( useInputFlags, USE_INPUT_ALT ) )
		return


		CustomUsePrompt_SetLastUsedTime( Time() )

	thread ReceiverActivate_LongPress_Thread( receiver, player )
}

void function ReceiverActivate_LongPress_Thread( entity receiver, entity player )
{
	ExtendedUseSettings settings
	settings.duration = tuning.useTime


		settings.loopSound = "survival_titan_linking_loop"
		settings.displayRuiType = eExtendedUseRuiType.NONE
		settings.displayRui = $"ui/extended_use_hint.rpak"
		settings.displayRuiFunc = void function( entity ent, entity player, var rui, ExtendedUseSettings settings )
		{
			RuiSetString( rui, "holdButtonHint", settings.holdHint )
			RuiSetString( rui, "hintText", settings.hint )
			RuiSetGameTime( rui, "startTime", Time() )
			RuiSetGameTime( rui, "endTime", Time() + settings.duration )
		}
		settings.icon = $""
		settings.hint = Localize( "#TRANSPORT_PORTAL_CHASE_PORTAL_CONFIRM_PROMPT" )
		file.useStartTime = Time()







		thread SetUseStartTime_Thread( player )


	waitthread ExtendedUse( receiver, player, settings )


		player.Signal( TRANSPORT_PORTAL_LONG_HOLD_END )

}


void function SetUseStartTime_Thread( entity player )
{
	EndSignal( player, TRANSPORT_PORTAL_LONG_HOLD_END )

	OnThreadEnd(
		function() : ()
		{
			file.useStartTime = -1
		}
	)

	
	
	WaitEndFrame()
	file.useStartTime = Time()

	WaitForever()
}
















string function Receiver_TextOverride( entity ent )
{
	return "#TRANSPORT_PORTAL_ALLY_RECALL_USE_PROMPT_LOOKING"
}










































































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________Receiver_UI___________________________(){}
#endif

bool function InRangeOfReceiver( entity player, entity receiver )
{
	if ( Distance( receiver.GetOrigin(), player.GetOrigin() ) > tuning.maxUseDistance )
		return false

	return true
}


void function TransportPortalCreatedHint_Thread( entity receiver, entity player, float hintDuration )
{
	Assert ( IsNewThread(), "Must be started as new thread" )

	Assert ( IsValid( receiver ) && IsValid( player ), "calling TransportPortalCreatedHint_Thread with invalid ents!" )

	receiver.Signal( TRANSPORT_PORTAL_HINT_THREAD )
	player.EndSignal( "OnDeath", "OnDestroy" )
	receiver.EndSignal( "OnDestroy", "OnBeginDissolve", TRANSPORT_PORTAL_HINT_THREAD )

	float removeHintTime   = Time() + hintDuration

	AddPlayerHint( hintDuration, 0, TRANSPORT_PORTAL_RECEIVER_IMAGE, Localize( "#TRANSPORT_PORTAL_ALLY_RECALL_HINT" ) )

	OnThreadEnd(
		function() : ()
		{
			HidePlayerHint( Localize( "#TRANSPORT_PORTAL_ALLY_RECALL_HINT" ) )
		}
	)

	while ( Time() < removeHintTime )
	{
		if ( player.GetUseEntity() == receiver )
			return

		WaitFrame()
	}
}

void function TransportPortalCreatedHintKnocked_Thread( entity receiver, entity player )
{
	Assert ( IsNewThread(), "Must be started as new thread" )

	Assert ( IsValid( receiver ) && IsValid( player ), "calling TransportPortalCreatedHint_Thread with invalid ents!" )

	receiver.Signal( TRANSPORT_PORTAL_HINT_THREAD )
	player.EndSignal( "OnDeath", "OnDestroy" )
	receiver.EndSignal( "OnDestroy", "OnBeginDissolve", TRANSPORT_PORTAL_HINT_THREAD )

	const float hintDuration = 100

	PassByReferenceString currentHint
	currentHint.value = ""

	string knockedHint = Localize( "#TRANSPORT_PORTAL_ALLY_RECALL_HINT_KNOCKED" )
	string outOfRangeHint = Localize( "#TRANSPORT_PORTAL_ALLY_RECALL_USE_PROMPT_NOT_LOOKING_OUT_OF_RANGE" )


	OnThreadEnd(
		function() : ( currentHint )
		{
			if ( currentHint.value != "" )
			{
				HidePlayerHint( currentHint.value )
			}
		}
	)

	while ( Bleedout_IsBleedingOut( player ) )
	{
		if ( player.GetUseEntity() == receiver )
		{
			if ( currentHint.value != "" )
			{
				HidePlayerHint( currentHint.value )
				currentHint.value = ""
			}
		}
		else if ( !InRangeOfReceiver( player, receiver ) )
		{
			if ( currentHint.value != outOfRangeHint )
			{
				HidePlayerHint( currentHint.value )
				currentHint.value = outOfRangeHint
				AddPlayerHint( hintDuration, 0, TRANSPORT_PORTAL_RECEIVER_IMAGE, currentHint.value )
			}
		}
		else if ( IsValid( GetReceiverForTryChannel( player ) ) )
		{
			if ( currentHint.value != knockedHint )
			{
				HidePlayerHint( currentHint.value )
				currentHint.value = knockedHint
				AddPlayerHint( hintDuration, 0, TRANSPORT_PORTAL_RECEIVER_IMAGE, currentHint.value )
			}
		}
		else
		{
			if ( currentHint.value != "" )
			{
				HidePlayerHint( currentHint.value )
				currentHint.value = ""
			}
		}


		WaitFrame()
	}
}



void function OnBleedoutStarted( entity victim, float endTime )
{
	if ( victim != GetLocalViewPlayer() )
		return

	int team = victim.GetTeam()
	if ( !( team in file.teamToReceiversMap ) )
		return

	Assert( file.teamToReceiversMap[team].len() > 0, "No receivers in the map!" )

	foreach ( entity receiver in file.teamToReceiversMap[team] )
	{
		if ( !IsValid( receiver ) )
			continue

		entity rootEnt = receiver.GetOwner()
		if ( !IsValid( rootEnt ) )
			continue

		if ( rootEnt.GetUseStateByIndex( victim.GetPlayerIndex() ) )
			continue

		thread DelayUltHintThreadOnKnock( victim, receiver )
		break
	}
}

void function DelayUltHintThreadOnKnock( entity victim, entity receiver )
{
	EndSignal( victim, "OnDeath", "OnDestroy" )
	EndSignal( receiver, "OnDestroy", "OnBeginDissolve" )

	wait 1.5

	if ( tuning.canUseWithCharacterActionWhenKnocked )
	{
		thread TransportPortalCreatedHintKnocked_Thread( receiver, victim )
	}
	else
	{
		thread TransportPortalCreatedHint_Thread( receiver, victim, 10 )
	}
}




void function OnYouRespawned()
{
	ArrayRemoveInvalid( file.allReceivers )

	int team = GetLocalViewPlayer().GetTeam()
	foreach( entity receiver in file.allReceivers )
	{
		if ( receiver.GetTeam() == team )
		{
			thread ManageLookAtRui_Thread( receiver, GetLocalViewPlayer() )
		}
	}
}



void function OnSpectatorTargetChanged( entity player, entity prevTarget, entity newTarget )
{
	if ( IsValid(prevTarget) )
	{
		prevTarget.Signal( TRANSPORT_PORTAL_SPECTATOR_CHANGED )
	}

	if ( !IsValid(newTarget) )
		return

	ArrayRemoveInvalid( file.allReceivers )

	int team = newTarget.GetTeam()
	foreach( entity receiver in file.allReceivers )
	{
		if ( receiver.GetTeam() == team )
		{
			thread ManageLookAtRui_Thread( receiver, GetLocalViewPlayer() )
		}
	}
}



void function ManageLookAtRui_Thread( entity receiver, entity player )
{
	Assert ( IsNewThread(), "Must be started as new thread" )

	if ( !IsValid( player ) || !IsValid( receiver ) )
		return

	player.EndSignal( "OnDeath", "OnDestroy", TRANSPORT_PORTAL_SPECTATOR_CHANGED )
	receiver.EndSignal( "OnDestroy", "OnBeginDissolve" )

	entity rootEnt = receiver.GetOwner()

	if ( !IsValid( rootEnt )  )
		return

	rootEnt.EndSignal( "OnDestroy" )

	file.ultPendingRuiDurationUpdate = null

	vector pos             = receiver.GetOrigin()

	var iconRui            = CreateCockpitPostFXRui( TRANSPORT_PORTAL_IN_WORLD_HUD_OBJECT, RuiCalculateDistanceSortKey( player.EyePosition(), pos ) )
	var regroupInfoRui	   = CreateCockpitPostFXRui( $"ui/alter_ult_hint_display.rpak", RuiCalculateDistanceSortKey( player.EyePosition(), pos ) )

	RuiTrackFloat3( iconRui, "pos", receiver, RUI_TRACK_ABSORIGIN_FOLLOW  )
	RuiTrackFloat3( iconRui, "playerAngles", player, RUI_TRACK_CAMANGLES_FOLLOW )
	RuiKeepSortKeyUpdated( iconRui, true, "pos" )

	RuiSetBool( iconRui, "hideDestroyText", !tuning.allowRemotePackup  )

	OnThreadEnd(
		function() : ( iconRui, regroupInfoRui )
		{
			if ( iconRui != null )
			{
				RuiDestroyIfAlive( iconRui )
			}

			if ( regroupInfoRui != null )
			{
				RuiDestroyIfAlive( regroupInfoRui )
			}
		}
	)

	RuiTrackFloat3( regroupInfoRui, "pos", receiver, RUI_TRACK_ABSORIGIN_FOLLOW  )

	float endTime = player.GetPlayerNetTime( TRANSPORT_PORTAL_EXPIRE_TIME_NETVAR )
	if ( endTime == 0 )
	{
		RuiSetBool( regroupInfoRui, "isEndless", true  )
		RuiSetBool( iconRui, "isEndless", true )
	}
	else
	{
		RuiSetGameTime( regroupInfoRui, "endTime", endTime  )
		RuiSetGameTime( iconRui, "endTime", endTime )
	}

	while ( true )
	{
		bool inRange = InRangeOfReceiver( player, rootEnt)
		bool hasPlayerUsedRecal = rootEnt.GetUseStateByIndex( player.GetPlayerIndex() )

		RuiSetBool( iconRui, "isInRange", inRange )
		RuiSetBool( regroupInfoRui, "isInRange", inRange )

		RuiSetBool( regroupInfoRui, "playerHasUsedRecall", hasPlayerUsedRecal )

		if ( file.ultPendingRuiDurationUpdate == receiver )
		{
			file.ultPendingRuiDurationUpdate = null
			endTime = player.GetPlayerNetTime( TRANSPORT_PORTAL_EXPIRE_TIME_NETVAR )
			if ( endTime == 0 )
			{
				RuiSetBool( regroupInfoRui, "isEndless", true  )
				RuiSetBool( iconRui, "isEndless", true )
			}
			else
			{
				RuiSetGameTime( regroupInfoRui, "endTime", endTime  )
				RuiSetGameTime( iconRui, "endTime", endTime )
			}
		}

		bool ownedByPlayer = rootEnt.GetOwner() == player
		bool isChanneling = TransportPortal_IsPlayerCurrentlyChanneling( player )
		bool showText = false

		if (  hasPlayerUsedRecal )
		{
			showText = ownedByPlayer
		}
		else if ( isChanneling || player.p.isInExtendedUse )
		{
			if ( player.p.lastExtededUseEnt == receiver )
				showText = true
		}
		else
		{
			entity useEnt = player.GetUseEntity()
			if ( useEnt == receiver )
			{
				showText = true
			}
			else if ( !inRange && useEnt == null )
			{
				showText = true
			}
		}

		RuiSetBool( regroupInfoRui, "isVisible", showText )

		RuiSetBool( iconRui, "isVisible",  inRange && !hasPlayerUsedRecal )

		RuiSetBool( regroupInfoRui, "isChanneling", isChanneling )
		RuiSetBool( iconRui, "isChanneling", isChanneling )

		
		int team                   = player.GetTeam()
		int squadUses			   = 0 

		if (rootEnt.GetUseStateByIndex( player.GetPlayerIndex() ))
			squadUses++

		array<entity> playerSquad = GetPlayerArrayOfTeam( team )
		ArrayRemoveInvalid( playerSquad )

		RuiSetInt( regroupInfoRui, "squadSize", playerSquad.len() )
		RuiSetInt( iconRui, "squadSize", playerSquad.len() )

		for ( int index = 0 ; index < playerSquad.len() ; index++ )
		{
			if( index > 3 )
				break

			RuiSetBool( iconRui, "isOwner", ownedByPlayer )
			RuiSetBool( regroupInfoRui, "isOwner", ownedByPlayer )

			if ( index < playerSquad.len() && index > 0)
			{
				entity squadMate = playerSquad[index]
				ItemFlavor character = LoadoutSlot_GetItemFlavor( ToEHI( squadMate ), Loadout_Character() )
				RuiSetImage( iconRui, "teammatePortrait" + index, CharacterClass_GetGalleryPortrait(character) )
				RuiSetBool( iconRui, "teammateHasUsed" + index, rootEnt.GetUseStateByIndex( squadMate.GetPlayerIndex() ) )

				if (rootEnt.GetUseStateByIndex( squadMate.GetPlayerIndex() ))
					squadUses++
			}
		}

		RuiSetInt( iconRui, "squadUses", squadUses )

		float fillAmount = 0
		if ( file.useStartTime > 0 )
		{
			fillAmount = clamp((Time() - file.useStartTime) / tuning.useTime, 0, 1)
		}

		if ( isChanneling && file.channelStartTime == -1 )
			file.channelStartTime = Time()

		if ( !isChanneling )
			file.channelStartTime = -1

		if ( file.channelStartTime > 0 && file.channelTime > 0 )
		{
			fillAmount = clamp((Time() - (file.channelStartTime - file.channelTimeElapsed)) / file.channelTime, 0, 1)
		}


		RuiSetFloat( iconRui, "arcFill", fillAmount )
		RuiSetFloat( regroupInfoRui, "arcFill", fillAmount )

		WaitFrame()
	}
}



void function ServerToClient_TransportPortal_UltDurationChanged( entity ult )
{
	file.ultPendingRuiDurationUpdate = ult
}


entity function TransportPortal_GetReceiverPlayerIsLookingAt( entity player )
{
	if ( !IsValid( player ) )
		return null

	int team = player.GetTeam()
	if ( !( team in file.teamToReceiversMap ) )
		return null

	vector eyeDir = player.GetViewVector()
	vector eyePos = player.EyePosition()
	const float MAX_DOT = cos( 3.0 * DEG_TO_RAD )

	entity bestReceiver = null
	float closestDist = FLOAT_INFINITY
	foreach( entity receiver in file.teamToReceiversMap[team] )
	{
		if ( !IsValid( receiver ) )
			continue

		vector playerToReceiver = (receiver.GetOrigin() - eyePos)
		float distToReceiver = Length( playerToReceiver )
		vector dirFromPlayerToReceiver = playerToReceiver / distToReceiver

		if ( DotProduct( eyeDir, dirFromPlayerToReceiver ) > MAX_DOT )
		{
			if ( distToReceiver < closestDist )
			{
				bestReceiver = receiver
			}
		}
	}

	return bestReceiver
}

#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________ChasePortal___________________________(){}
#endif



































































































































































void function ChasePortalLifetime_Client_Thread( entity allyPortalRootEnt )
{
	AllyPortalData data
	file.allyPortalToDataMap[allyPortalRootEnt] <- data

	EndSignal( allyPortalRootEnt, "OnDestroy" )

	OnThreadEnd(
		function() : ( allyPortalRootEnt )
		{
			delete file.allyPortalToDataMap[allyPortalRootEnt]
		}
	)

	WaitForever()
}








































































































































































































































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________PathGeneration___________________________(){}
#endif


















































































































































































































































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________IncomingPlayerWarning___________________________(){}
#endif






















































































bool function IsPlayersBeingWarnedAboutNexus( entity player )
{
	array<entity> closeUlts = GetEntitiesFromArrayNearPos( file.warningTriggers, player.GetOrigin(), tuning.incomingWarningRadius )
	foreach ( entity trigger in closeUlts )
	{
		if ( IsEnemyTeam( player.GetTeam(), trigger.GetTeam() ) && trigger.GetOwner() != player )
		{
			return true
		}
	}
	return false
}

void function TransportPortal_SetChannelUseTime(float channelTime, float channelTimeElapsed)
{
	file.channelTime = channelTime
	file.channelTimeElapsed = channelTimeElapsed
}

void function CleanupWarningTriggerOnDestroy_Thread( entity trigger )
{
	EndSignal( trigger, "OnDestroy" )

	OnThreadEnd(
		function() : ( trigger )
		{
			file.warningTriggers.fastremovebyvalue( trigger )
		}
	)

	WaitForever()
}


#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________ChasePortalWaypoints___________________________(){}
#endif





































































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________MiniMap___________________________(){}
#endif


const vector RING_COLOR = ALTER_PURPLE_COLOR/255
const float  RING_PLAYER_ALPHA = 1.0
const float  RING_SPECTATOR_ALPHA = 0.2

void function SetupMapRuiReceiverPreview( entity receiverProxy, var rui, bool isFullMap )
{
	RuiSetAsset( rui, "areaImage", $"rui/hud/character_abilities/transport_portal_map_circle" )
	RuiSetFloat( rui, "areaImageAlpha",
		GetLocalClientPlayer().GetTeam() == TEAM_SPECTATOR ? RING_SPECTATOR_ALPHA : RING_PLAYER_ALPHA )
	RuiSetImage( rui, "clampedImage", $"" )

	string areaColorArgName = "objColor"
	if ( !isFullMap )
	{
		RuiSetBool( rui, "useOverrideColor", true )
		areaColorArgName = "overrideColor"
	}
	RuiSetColorAlpha( rui, areaColorArgName, RING_COLOR, 1 )

	RuiSetImage( rui, "centerImage", TRANSPORT_PORTAL_RECEIVER_IMAGE )
	RuiSetFloat( rui, "objectRadius", tuning.maxUseDistance / 16384.0 )

	if ( IsValid( receiverProxy ) )
	{
		thread RecieverPreview_Thread( receiverProxy, rui, areaColorArgName )
	}
}

void function SetupMapRuiReceiver( entity receiver, var rui, bool isFullMap )
{
	RuiSetAsset( rui, "areaImage", $"rui/hud/character_abilities/transport_portal_map_circle" )
	RuiSetFloat( rui, "areaImageAlpha",
		GetLocalClientPlayer().GetTeam() == TEAM_SPECTATOR ? RING_SPECTATOR_ALPHA : RING_PLAYER_ALPHA )
	RuiSetImage( rui, "clampedImage", $"" )

	string areaColorArgName = "objColor"
	if ( !isFullMap )
	{
		RuiSetBool( rui, "useOverrideColor", true )
		areaColorArgName = "overrideColor"
	}
	RuiSetColorAlpha( rui, areaColorArgName, RING_COLOR, 1 )

	RuiSetImage( rui, "centerImage", $"" )
	RuiSetFloat( rui, "objectRadius", tuning.maxUseDistance / 16384.0 )

	if ( IsValid( receiver ) )
	{
		thread InRangeForMinimapRui_Thread( receiver, rui, areaColorArgName )
	}
}
void function SetupMapRuiTranslocator( entity ent, var rui )
{
	RuiSetImage( rui, "defaultIcon", TRANSPORT_PORTAL_RECEIVER_IMAGE )
	RuiSetImage( rui, "clampedDefaultIcon", TRANSPORT_PORTAL_RECEIVER_IMAGE )
	RuiSetBool( rui, "useTeamColor", false )
	RuiSetFloat( rui, "iconBlend", 0.0 )
}

void function SetupMapRuiChasePortal( entity ent, var rui )
{
	RuiSetImage( rui, "defaultIcon", TRANSPORT_PORTAL_PORTAL_ICON )
	RuiSetImage( rui, "clampedDefaultIcon", TRANSPORT_PORTAL_PORTAL_ICON )
	RuiSetBool( rui, "useTeamColor", false )
	RuiSetFloat( rui, "iconBlend", 0.0 )
}

void function InRangeForMinimapRui_Thread( entity receiver, var rui, string argName )
{
	EndSignal( receiver, "OnDestroy", "OnBeginDissolve" )

	while ( true )
	{
		entity player = GetLocalViewPlayer()
		bool playerInRing = false
		entity rootEnt = receiver.GetOwner()
		if ( IsValid( player ) && IsValid( rootEnt ) )
		{
			if ( ( rootEnt.GetNetworkedClassName() == "prop_material_harvester" ) && !rootEnt.GetUseStateByIndex( player.GetPlayerIndex() ) )
			{
				playerInRing = Distance( player.GetOrigin(), receiver.GetOrigin() ) < tuning.maxUseDistance
			}
		}
		
		vector colour = playerInRing ? <1,1,1> : <0.1, 0.1, 0.1>
		float alpha = playerInRing ? 1.0 : 0.1
		RuiSetColorAlpha( rui, argName, colour, alpha )
		WaitFrame()
	}
}

void function RecieverPreview_Thread( entity receiverProxy, var rui, string argName )
{
	EndSignal( receiverProxy, "OnDestroy" )

	while ( true )
	{
		vector receieverPos = receiverProxy.GetOrigin()
		bool posInNextRing = Cl_SURVIVAL_PosInSafeZone( receieverPos )
		bool posOutsideRing = !Cl_SURVIVAL_PosInsideDeathField( receieverPos )
		float blinkRate = posOutsideRing ? 2.5 : !posInNextRing ? 1.3 : 0.5
		float v = 0.8 + 0.6 * sin( blinkRate * 2 * PI * Time() )
		RuiSetColorAlpha( rui, argName, <v, v, v>, v )
		WaitFrame()
	}
}


      