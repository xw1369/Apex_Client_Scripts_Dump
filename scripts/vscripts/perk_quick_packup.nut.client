global function Perk_QuickPackup_Init
global function Perk_QuickPackup_Enabled
global function Perk_QuickPackup_RemoteInteractEnabled
global function Perk_QuickPackup_GetRemotePickupBlockRange












global function Perk_QuickPackup_NagThread
global function ServerToClient_Perk_QuickPackup_Register
global function ServerToClient_Perk_QuickPackup_RegisterUsable
global function Perk_QuickPackup_Cancel



global function Perk_QuickPackup_IsRemotePickup
global function Perk_QuickPackup_ShoudUseBlockReload


struct
{






	array< int > placableHandles
	array< int > usableHandles
	float lastNagTime = 0.0
	bool abandonedThreadStarted
	float lastAbandonedStartTime
	float lastAbandonedInputTime

	bool isChanneling
} file

struct
{
	bool useRemoteInteract = true
	bool doAbandonedPickup = true
	float pickupRange = 3000.0
	float mediumAbandonRange = 6000.0
	float largeAbandonRange = 12000.0
	float nagDelayTime = -1.0
	float abandonedPickupTime = 10
	float remotePickupThreshold = 200
	float rampartPickupBlockRange = 300
}tunings

const string QUICK_PACKUP_SOUND = "Controller_Recall_1p"
const string QUICK_PACKUP_CLIENT_TO_SERVER_START = "ClientToServer_Perk_QuickPackup_Start"
const string QUICK_PACKUP_CLIENT_TO_SERVER_CANCEL = "ClientToServer_Perk_QuickPackup_Cancel"
const string QUICK_PACKUP_CLIENT_TO_SERVER_SINGLE_TRAP = "ClientToServer_Perk_QuickPackup_SingleTrap"
const string QUICK_PACKUP_CLIENT_TO_SERVER_SINGLE_TRAP_CATALYST = "ClientToServer_Perk_QuickPackup_SingleTrap_Catalyst"
const string QUICK_PACKUP_CLIENT_TO_SERVER_FURTHEST_PICKUP = "ClientToServer_Perk_QuickPackup_FurthestPlacable"
const string QUICK_PACKUP_CLIENT_TO_SERVER_ABANDONED_PICKUP = "ClientToServer_Perk_QuickPackup_AbandonedPickup"
const string QUICK_PACKUP_SERVER_TO_CLIENT_REGISTER = "ServerToClient_Perk_QuickPackup_Register"
const string QUICK_PACKUP_SERVER_TO_CLIENT_REGISTER_USABLE = "ServerToClient_Perk_QuickPackup_RegisterUsable"
const string QUICK_PACKUP_ENDED_SIGNAL = "quick_packup_ended"
const float AR_EFFECT_SIZE = 768.0 
const vector AR_COLOR = <60, 110, 300>
const asset RADIUS_FX = $"P_ar_edge_ring_gen"


void function Perk_QuickPackup_Init()
{

		Remote_RegisterServerFunction( QUICK_PACKUP_CLIENT_TO_SERVER_START )
		Remote_RegisterServerFunction( QUICK_PACKUP_CLIENT_TO_SERVER_CANCEL )
		Remote_RegisterServerFunction( QUICK_PACKUP_CLIENT_TO_SERVER_SINGLE_TRAP, "typed_entity", "prop_script" )
		Remote_RegisterServerFunction( QUICK_PACKUP_CLIENT_TO_SERVER_SINGLE_TRAP_CATALYST, "typed_entity", "prop_ferro_prop" )
		Remote_RegisterServerFunction( QUICK_PACKUP_CLIENT_TO_SERVER_FURTHEST_PICKUP )
		Remote_RegisterServerFunction( QUICK_PACKUP_CLIENT_TO_SERVER_ABANDONED_PICKUP )
		Remote_RegisterClientFunction( QUICK_PACKUP_SERVER_TO_CLIENT_REGISTER, "int",INT_MIN, INT_MAX )
		Remote_RegisterClientFunction( QUICK_PACKUP_SERVER_TO_CLIENT_REGISTER_USABLE, "int",INT_MIN, INT_MAX )
		RegisterSignal( QUICK_PACKUP_ENDED_SIGNAL )


	if( !Perk_QuickPackup_Enabled() )
		return


	RegisterConCommandTriggeredCallback( "+scriptCommand5", Perk_QuickPackup_Client )



	if( tunings.useRemoteInteract )
		AddGetExtendedRangeUseEntityCallback( GetExtendedRangeUseEntityCallbackForPlayer_QuickPickup )

	AddCallback_OnPlayerWeaponDryFired( Perk_QuickPackup_OnWeaponDryFire )


	tunings.pickupRange = GetCurrentPlaylistVarFloat( "perk_quick_packup_range", tunings.pickupRange )
	tunings.mediumAbandonRange = GetCurrentPlaylistVarFloat( "perk_quick_medium_abandon_range", tunings.mediumAbandonRange )
	tunings.largeAbandonRange = GetCurrentPlaylistVarFloat( "perk_quick_large_abandon_range", tunings.largeAbandonRange )
	tunings.nagDelayTime = GetCurrentPlaylistVarFloat( "perk_quick_packup_nag_delay_time", tunings.nagDelayTime )
	tunings.useRemoteInteract = GetCurrentPlaylistVarBool( "perk_quick_packup_use_remote_interact", tunings.useRemoteInteract )
	tunings.abandonedPickupTime = GetCurrentPlaylistVarFloat( "perk_quick_packup_abandoned_pickup_time", tunings.abandonedPickupTime )
	tunings.remotePickupThreshold = GetCurrentPlaylistVarFloat( "perk_quick_packup_remote_pickup_threshold", tunings.remotePickupThreshold )
	tunings.rampartPickupBlockRange = GetCurrentPlaylistVarFloat( "perk_quick_packup_remote_pickup_block_range", tunings.rampartPickupBlockRange )
	tunings.doAbandonedPickup = GetCurrentPlaylistVarBool( "perk_quick_packup_do_abandoned_pickup", tunings.doAbandonedPickup )
}

bool function Perk_QuickPackup_Enabled()
{
	return Perks_S22UpdateEnabled() && GetCurrentPlaylistVarBool( "perk_quick_packup_enabled", true )
}

bool function Perk_QuickPackup_RemoteInteractEnabled()
{
	return Perk_QuickPackup_Enabled() && tunings.useRemoteInteract
}

float function Perk_QuickPackup_GetRemotePickupBlockRange()
{
	return tunings.rampartPickupBlockRange
}


void function Perk_QuickPackup_OnWeaponDryFire( entity player, entity weapon )
{
	entity tacWeapon = player.GetOffhandWeapon( OFFHAND_TACTICAL )
	if( weapon != tacWeapon )
		return

	int curTacAmmo       	= tacWeapon.GetWeaponPrimaryClipCount()
	int perShotAmmo 		= tacWeapon.GetAmmoPerShot()
	if( curTacAmmo > perShotAmmo )
		return








		if( file.usableHandles.len() == 0 )
			return
		
		file.lastAbandonedStartTime = Time()

}

bool function Perk_QuickPackup_ShoudUseBlockReload( entity player, entity ent )
{
	bool isRemotePickup = Perk_QuickPackup_IsRemotePickup( ent, player )
	return !isRemotePickup
}

bool function Perk_QuickPackup_IsRemotePickup( entity pickupEnt, entity owner )
{
	float distSqr = DistanceSqr( pickupEnt.GetOrigin(), owner.GetOrigin() )
	return distSqr > tunings.remotePickupThreshold * tunings.remotePickupThreshold
}

void function GetExtendedRangeUseEntityCallbackForPlayer_QuickPickup( entity player, array<entity> outEnts )
{
	if ( !Perks_DoesPlayerHavePerk( player, ePerkIndex.RING_EXPERT ) )
	{
		return
	}

	array<entity> playerPlacables








	playerPlacables = Perk_QuickPackup_GetEntArrFromHandles( file.usableHandles )


	if( playerPlacables.len() > 0 )
	{
		array<entity> nearbyPlacables = GetEntitiesFromArrayNearPos( playerPlacables, player.GetOrigin(), tunings.pickupRange )
		
		bool shouldCheckLos = PlayerHasPassive( player, ePassives.PAS_LOCKDOWN )
		foreach( entity ent in nearbyPlacables )
		{
			if( shouldCheckLos && !PlayerCanSee( player, ent, true, 10 ) )
			{
				continue
			}
			outEnts.append( ent )
		}
	}
}

float function GetAbandonedPickupRangeStart( entity player )
{
	if( PlayerHasPassive( player, ePassives.PAS_LOCKDOWN ) || PlayerHasPassive( player, ePassives.PAS_BATTERY_POWERED ) )
	{
		return tunings.mediumAbandonRange
	}
	else if( PlayerHasPassive( player, ePassives.PAS_GAS_GEAR ) )
	{
		return tunings.largeAbandonRange
	}

	return tunings.pickupRange
}

int function GetTacChargesNeeded( entity player )
{
	entity tacWeapon = player.GetOffhandWeapon( OFFHAND_TACTICAL )
	if( !IsValid( tacWeapon ) )
		return 0

	int curTacAmmo       	= tacWeapon.GetWeaponPrimaryClipCount()
	int maxAmmo = tacWeapon.GetWeaponPrimaryClipCountMax()
	int ammoNeeded = maxAmmo - curTacAmmo
	int perShotAmmo 		= tacWeapon.GetAmmoPerShot()
	return int( ceil( float( ammoNeeded ) / perShotAmmo ) )
}

void function FilterOutNonReclaimableTraps( entity player, array<entity> traps )
{
	array<entity> toRemove
	if( PlayerHasPassive( player, ePassives.PAS_LOCKDOWN ) )
	{
		foreach( ent in traps )
		{
			if( !CanReclaimSpikeTrap( ent ) )
			{
				toRemove.append( ent )
			}
		}
	}
	else if( PlayerHasPassive( player, ePassives.PAS_GUNNER ) )
	{
		foreach( ent in traps )
		{
			if( !CanReclaimWall( ent ) )
			{
				toRemove.append( ent )
				continue
			}
			if( IsEnemyBlockingRemoteReclaim( ent ) )
			{
				toRemove.append( ent )
				continue
			}
		}
	}
	foreach( ent in toRemove )
	{
		traps.fastremovebyvalue( ent )
	}
}

























































































































































void function ServerToClient_Perk_QuickPackup_Register( int encodedHandle )
{
	file.placableHandles.append( encodedHandle )
	thread QuickPackup_TryStartAbandonedThread()
}

void function QuickPackup_TryStartAbandonedThread()
{
	if( file.abandonedThreadStarted )
		return

	if( !tunings.doAbandonedPickup )
		return

	entity localPlayer = GetLocalViewPlayer()
	localPlayer.EndSignal( "OnDestroy", "OnDeath" )

	OnThreadEnd(
		function() : ()
		{
			file.abandonedThreadStarted = false
		}
	)

	while( true )
	{
		array<entity> placables = Perk_QuickPackup_GetEntArrFromHandles( file.placableHandles )
		FilterOutNonReclaimableTraps( localPlayer, placables )
		array<entity> nearbyEnts
		bool clearTime = true
		if( placables.len() > 0 )
		{
			nearbyEnts = GetEntitiesFromArrayNearPos( placables, localPlayer.GetOrigin(), GetAbandonedPickupRangeStart( localPlayer ) )
			if( nearbyEnts.len() == 0 )
			{
				if( file.lastAbandonedStartTime == -1 )
				{
					file.lastAbandonedStartTime = Time()
				}
				clearTime = false
			}
		}

		int chargesNeeded = GetTacChargesNeeded( localPlayer )

		bool abandonedTimeValid = file.lastAbandonedStartTime > 0 && Time() - file.lastAbandonedStartTime < tunings.abandonedPickupTime && placables.len() - nearbyEnts.len() > 0 && Time() - file.lastAbandonedInputTime > 3 && chargesNeeded > 0
		if( clearTime && !abandonedTimeValid )
		{
			file.lastAbandonedStartTime = -1
		}

		if( abandonedTimeValid )
		{
			AddPlayerHint( .2, 0, $"", Localize( "#QUICK_PACKUP_PROMPT", minint( placables.len() - nearbyEnts.len(), chargesNeeded ) ) )
		}

		WaitFrame()
	}
}

void function ServerToClient_Perk_QuickPackup_RegisterUsable( int encodedHandle )
{
	file.usableHandles.append( encodedHandle )
}

bool function Perk_QuickPackup_Client_RemoteInteract( entity player )
{
	entity useEnt = player.GetUsePromptEntity()
	if ( !IsValid( useEnt ) )
		return false

	int handle = useEnt.GetEncodedEHandle()
	if( !file.usableHandles.contains( handle ) )
		return false

	if( PlayerHasPassive( player, ePassives.PAS_LOCKDOWN ) )
	{
		Remote_ServerCallFunction( QUICK_PACKUP_CLIENT_TO_SERVER_SINGLE_TRAP_CATALYST, useEnt )
	}
	else
	{
		Remote_ServerCallFunction( QUICK_PACKUP_CLIENT_TO_SERVER_SINGLE_TRAP, useEnt )
	}
	return true
}

void function Perk_QuickPackup_Client( entity player )
{
	if( tunings.useRemoteInteract )
	{
		if ( IsControllerModeActive() )
		{
			if ( TryOnscreenPromptFunction( player, "quickchat" ) )
				return
		}

		entity activeWeapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
		
		entity ultWeapon = player.GetOffhandWeapon( OFFHAND_ULTIMATE )
		entity placementWeapon = player.GetOffhandWeapon( OFFHAND_RIGHT )
		if( ( activeWeapon == ultWeapon && IsValid( ultWeapon ) && ( ultWeapon.GetWeaponClassName() == MOBILE_HMG_WEAPON_NAME ) ) || ( activeWeapon == placementWeapon && IsValid( placementWeapon ) && placementWeapon.GetWeaponClassName() == MOUNTED_TURRET_PLACEABLE_WEAPON_NAME ) )
			return

		if( Perk_QuickPackup_Client_RemoteInteract( player ) )
		{
			return
		}

		if( file.lastAbandonedStartTime > 0 && Time() - file.lastAbandonedStartTime < tunings.abandonedPickupTime )
		{
			file.lastAbandonedStartTime = -1
			file.lastAbandonedInputTime = Time()
			EmitSoundOnEntity( player, QUICK_PACKUP_SOUND )
			Remote_ServerCallFunction( QUICK_PACKUP_CLIENT_TO_SERVER_ABANDONED_PICKUP )
		}
	}
}

void function Perk_QuickPackup_StartChannel( entity player )
{
	if( player.ContextAction_IsActive() )
		return
	if( Bleedout_IsBleedingOut( player ) )
		return

	if( !Perks_DoesPlayerHavePerk( player, ePerkIndex.RING_EXPERT ) )
		return

	if( !file.isChanneling )
	{
		thread Perk_QuickPackup_ConfirmationThread( player )
	}
}

void function Perk_QuickPackup_ConfirmationThread( entity player )
{
	player.EndSignal( "OnDeath", "OnDestroy", "BleedOut_OnStartDying", QUICK_PACKUP_ENDED_SIGNAL )
	file.isChanneling = true
	file.lastNagTime = Time()

	OnThreadEnd(
		function() : ()
		{
			file.isChanneling = false
		}
	)


	while( true )
	{
		AddPlayerHint( .1, 0, $"", "%attack% to pick up furthest placable" )
		WaitFrame()
	}

}

void function Perk_QuickPackup_Cancel( entity player )
{
	if( file.isChanneling )
	{
		player.Signal( QUICK_PACKUP_ENDED_SIGNAL )
		if( player.IsUserCommandButtonHeld( IN_ATTACK ) )
		{
			Remote_ServerCallFunction( QUICK_PACKUP_CLIENT_TO_SERVER_FURTHEST_PICKUP )
		}
		else
		{
			Remote_ServerCallFunction( QUICK_PACKUP_CLIENT_TO_SERVER_CANCEL )
		}
	}
}

void function Perk_QuickPackup_NagThread( entity player )
{
	player.EndSignal( "OnDestroy", "OnDeath", RING_EXPERT_END_SIGNAL )

	if( tunings.nagDelayTime < 0 )
		return

	while( Perks_DoesPlayerHavePerk( player, ePerkIndex.RING_EXPERT ) )
	{
		Wait( .5 )

		float curTime = Time()
		if( curTime - file.lastNagTime > tunings.nagDelayTime )
		{
			AddPlayerHint( 10, 0, $"", "%scriptCommand5% To Pickup Nearby Placables" )
			file.lastNagTime = curTime
		}
	}
}

array<entity> function Perk_QuickPackup_GetEntArrFromHandles( array<int> handles )
{
	array<entity> result
	array<int> handlesToRemove
	foreach( int handle in handles )
	{
		entity ent = GetEntityFromEncodedEHandle( handle )
		if( IsValid( ent ) )
		{
			result.append( ent )
		}
		else
		{
			handlesToRemove.append( handle )
		}
	}
	foreach( handle in handlesToRemove )
	{
		handles.fastremovebyvalue( handle )
	}
	return result
}


