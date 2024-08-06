global function MpAbilityTransportPortalDatapad_Init

global function OnWeaponActivate_ability_transport_portal_datapad
global function OnWeaponDeactivate_ability_transport_portal_datapad
global function OnWeaponPrimaryAttack_ability_transport_portal_datapad

global function TransportPortalDatapad_UseSpecialKnockedAnims





global function TransportPortal_CancelUse


global const string  TRANSPORT_PORTAL_DATAPAD_WEAPON_NAME = "mp_ability_transport_portal_datapad"




const string TRANSPORT_PORTAL_CANCEL_CHANNEL_SIGNAL = "alter_ult_cancel_channel"
const string TRANSPORT_PORTAL_INTERRUPT_CHANNEL_SIGNAL = "alter_ult_interrupt_channel"
global const string TRANSPORT_PORTAL_FINISHED_CHANNEL_SIGNAL = "alter_ult_finish_channel"

const string TRANSPORT_PORTAL_SPECTATOR_TARGET_CHANGED = "alter_ult_spectator_changed"



const asset TRANSPORT_PORTAL_PRE_TRAVEL_FX = $"P_alter_ulti_portal_channeling"
const asset TRANSPORT_PORTAL_WARP_1P = $"P_alter_ulti_portal_warp_FP"
const asset TRANSPORT_PORTAL_ROPE_FRIENDLY = $"P_alter_ulti_portal_dir_friendly"
const asset TRANSPORT_PORTAL_ROPE_ENEMY = $"P_alter_ulti_portal_dir_enemy"


const string TRANSPORT_PORTAL_TELEPORT_CONFIRM_1P = "Alter_Ult_UI_Teleport_Confirm_1p"
const string TRANSPORT_PORTAL_START_CHANNEL_SOUND_1P = "Alter_Ult_Teleport_Buildup_1p"
const string TRANSPORT_PORTAL_START_CHANNEL_SOUND_3P = "Alter_Ult_Teleport_Buildup_3p"
const string TRANSPORT_PORTAL_START_CHANNEL_KNOCKED_SOUND_1P = "Alter_Ult_Teleport_Buildup_Downed_1p"
const string TRANSPORT_PORTAL_START_CHANNEL_KNOCKED_SOUND_3P = "Alter_Ult_Teleport_Buildup_Downed_3p"
const string TRANSPORT_PORTAL_START_CHANNEL_TRANSLOCATOR_SOUND_3P = "Alter_Ult_Base_Alert_RemotelyUsed_3p"

const bool TRANSPORT_PORTAL_DATAPAD_DEBUG = true

struct{
	float warpDelayAfterUse = 2.0
	float warpDelayAfterUseAdditionalWhenKnocked = 0.0

	bool canCancel = true
	bool cancelOnDamage = false
	bool cancelOnNonBulletDamage = false
}tuning

struct
{
	table < entity, bool > userToChannelCompletedMap



}file

void function MpAbilityTransportPortalDatapad_Init()
{
	SetupTuning()

	PrecacheWeapon( TRANSPORT_PORTAL_DATAPAD_WEAPON_NAME )

	PrecacheParticleSystem( TRANSPORT_PORTAL_PRE_TRAVEL_FX )
	PrecacheParticleSystem( TRANSPORT_PORTAL_WARP_1P )
	PrecacheParticleSystem( TRANSPORT_PORTAL_ROPE_FRIENDLY )
	PrecacheParticleSystem( TRANSPORT_PORTAL_ROPE_ENEMY )

	RegisterSignal( TRANSPORT_PORTAL_CANCEL_CHANNEL_SIGNAL )
	RegisterSignal( TRANSPORT_PORTAL_INTERRUPT_CHANNEL_SIGNAL )
	RegisterSignal( TRANSPORT_PORTAL_FINISHED_CHANNEL_SIGNAL )


	RegisterSignal( TRANSPORT_PORTAL_SPECTATOR_TARGET_CHANGED )

	AddOnSpectatorTargetChangedCallback( OnSpectatorTargetChanged )

}


void function SetupTuning()
{
	tuning.warpDelayAfterUse = 						GetCurrentPlaylistVarFloat	( "alter_ult_warpDelayAfterUse", tuning.warpDelayAfterUse )
	tuning.warpDelayAfterUseAdditionalWhenKnocked = GetCurrentPlaylistVarFloat	( "alter_ult_warpDelayAfterUseKnocked", tuning.warpDelayAfterUseAdditionalWhenKnocked )

	tuning.cancelOnDamage =							GetCurrentPlaylistVarBool	( "alter_ult_cancelOnDamage", tuning.cancelOnDamage )
	tuning.cancelOnNonBulletDamage =				GetCurrentPlaylistVarBool	( "alter_ult_cancelOnNonBulletDamage", tuning.cancelOnNonBulletDamage )
}

bool function TransportPortalDatapad_UseSpecialKnockedAnims()
{
	return tuning.warpDelayAfterUseAdditionalWhenKnocked > 0
}


void function OnSpectatorTargetChanged( entity player, entity prevTarget, entity newTarget )
{
	if ( IsValid( prevTarget ) && prevTarget.IsPlayer() )
	{
		entity weapon = prevTarget.GetOffhandWeapon( OFFHAND_EQUIPMENT )
		if ( IsValid( weapon ) && weapon.GetWeaponClassName() == TRANSPORT_PORTAL_DATAPAD_WEAPON_NAME )
		{
			if ( prevTarget in file.userToChannelCompletedMap )
			{
				delete file.userToChannelCompletedMap[prevTarget]
			}

			prevTarget.Signal( TRANSPORT_PORTAL_SPECTATOR_TARGET_CHANGED )
		}
	}

	if ( IsValid( newTarget ) && newTarget.IsPlayer() )
	{
		entity weapon = newTarget.GetOffhandWeapon( OFFHAND_EQUIPMENT )
		if ( IsValid( weapon ) && weapon.GetWeaponClassName() == TRANSPORT_PORTAL_DATAPAD_WEAPON_NAME )
		{
			file.userToChannelCompletedMap[newTarget] <- false

			bool playerIsKnocked = Bleedout_IsBleedingOut( newTarget )
			
			SetupFxAndSound( newTarget, weapon.GetWeaponChargeFraction(), false, playerIsKnocked )
		}
	}
}


void function OnWeaponActivate_ability_transport_portal_datapad( entity weapon )
{

		if ( weapon.GetOwner() != GetLocalViewPlayer() )
			return


#if TRANSPORT_PORTAL_DATAPAD_DEBUG
	printf( "Alter - " + weapon.GetOwner() + ": ACTIVATE portal datapad" )
#endif

	entity player = weapon.GetWeaponOwner()
	if ( !IsValid(player) )
		return

	file.userToChannelCompletedMap[player] <- false

	bool playerIsKnocked = Bleedout_IsBleedingOut( player )










	EmitSoundOnEntity( player, TRANSPORT_PORTAL_TELEPORT_CONFIRM_1P )
	SetupFxAndSound( player, weapon.GetWeaponChargeFraction(), true, playerIsKnocked )

}

void function OnWeaponDeactivate_ability_transport_portal_datapad( entity weapon )
{

	if ( weapon.GetOwner() != GetLocalViewPlayer() )
		return


#if TRANSPORT_PORTAL_DATAPAD_DEBUG
	printf( "Alter - " + weapon.GetOwner() + ": Data pad DEACTIVATE" )
#endif

	entity player = weapon.GetWeaponOwner()
	if ( !IsValid(player) )
		return

	if ( !( player in file.userToChannelCompletedMap ) || !file.userToChannelCompletedMap[player]  )
	{
		player.Signal( TRANSPORT_PORTAL_CANCEL_CHANNEL_SIGNAL )
#if TRANSPORT_PORTAL_DATAPAD_DEBUG
		printf("Alter - " + weapon.GetOwner() + ": datapad WAS CANCELLED " + weapon.GetWeaponChargeFraction() )
#endif
	}
	else
	{
		player.Signal( TRANSPORT_PORTAL_FINISHED_CHANNEL_SIGNAL )
	}

	if ( player in file.userToChannelCompletedMap  )
	{
		delete file.userToChannelCompletedMap[player]
	}





}


































var function OnWeaponPrimaryAttack_ability_transport_portal_datapad( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity player = weapon.GetWeaponOwner()
	if ( !IsValid( player ) )
		return 0

	if ( weapon.GetWeaponChargeFraction() < 1.0 )
		return 0

	file.userToChannelCompletedMap[player] <- true





















	return 1
}

string function GetChannelSFX( bool isKnocked, bool get1pSound )
{
	if ( isKnocked && TransportPortalDatapad_UseSpecialKnockedAnims() )
		return get1pSound ? TRANSPORT_PORTAL_START_CHANNEL_KNOCKED_SOUND_1P : TRANSPORT_PORTAL_START_CHANNEL_KNOCKED_SOUND_3P

	return get1pSound ? TRANSPORT_PORTAL_START_CHANNEL_SOUND_1P : TRANSPORT_PORTAL_START_CHANNEL_SOUND_3P
}










































































































































































void function SetupFxAndSound( entity player, float chargeFrac, bool forceSoundFromStart, bool playerIsKnocked )
{
	float channelTime = tuning.warpDelayAfterUse + (playerIsKnocked ? tuning.warpDelayAfterUseAdditionalWhenKnocked : 0.0)
	float channelTimeElapsed = chargeFrac * channelTime
	float soundTotalTime = channelTime
	string sound = GetChannelSFX( playerIsKnocked, true )

	if ( forceSoundFromStart )
	{
		EmitSoundOnEntity( player, sound )
	}
	else
	{
		soundTotalTime -= channelTimeElapsed
		EmitSoundOnEntityWithSeek( player, sound, channelTimeElapsed )
	}

	thread CleanupChannelSound_Thread( player, soundTotalTime, sound )
	thread PlayScreenFXWarpJumpTransportPortal( player )
	TransportPortal_SetChannelUseTime( channelTime, channelTimeElapsed )
}



void function CleanupChannelSound_Thread( entity player, float delay, string sound )
{
	player.EndSignal( "OnDeath", "OnDestroy", "BleedOut_OnStartDying", TRANSPORT_PORTAL_CANCEL_CHANNEL_SIGNAL, TRANSPORT_PORTAL_SPECTATOR_TARGET_CHANGED )

	OnThreadEnd(
		function() : ( player, sound )
		{
			StopSoundOnEntity( player, sound )
		}
	)

	wait delay
}



void function Channel_DisplayProgressBar( entity player, float channelTime, float channelTimeElapsed )
{
	player.EndSignal( "OnDeath", "OnDestroy", "BleedOut_OnStartDying", TRANSPORT_PORTAL_CANCEL_CHANNEL_SIGNAL, TRANSPORT_PORTAL_SPECTATOR_TARGET_CHANGED )

	string consumableName = "#ABL_ULT_TRANSPORT_PORTAL_SHORT"
	asset hudIcon         = $"rui/hud/ultimate_icons/ultimate_alter"
	float raiseTime       = 0
	float chargeTime      = channelTime

	var rui = CreateFullscreenRui( $"ui/consumable_progress.rpak" )

	RuiSetGameTime( rui, "healStartTime", ( Time() - channelTimeElapsed ) )
	RuiSetString( rui, "consumableName", consumableName )
	RuiSetFloat( rui, "raiseTime", raiseTime )
	RuiSetFloat( rui, "chargeTime", chargeTime )
	RuiSetImage( rui, "hudIcon", hudIcon )

	OnThreadEnd(
		function() : ( rui, player )
		{
			RuiDestroy( rui )
		}
	)

	RuiSetString( rui, "hintController", "#SURVIVAL_CANCEL_HEAL_GAMEPAD" )
	RuiSetString( rui, "hintKeyboardMouse", "#SURVIVAL_CANCEL_HEAL_PC" )

	wait ( channelTime - channelTimeElapsed )
}



void function PlayScreenFXWarpJumpTransportPortal( entity player )
{
	Assert ( IsNewThread(), "Must be started as new thread" )

	player.EndSignal( "OnDeath", "OnDestroy", "BleedOut_OnStartDying", TRANSPORT_PORTAL_CANCEL_CHANNEL_SIGNAL, TRANSPORT_PORTAL_SPECTATOR_TARGET_CHANGED )

	int fxHandle = -1
	if ( IsValid( player.GetCockpit() ) )
	{
		fxHandle = StartParticleEffectOnEntity( player, GetParticleSystemIndex( TRANSPORT_PORTAL_WARP_1P ), FX_PATTACH_POINT_FOLLOW, player.GetCockpit().LookupAttachment( "CAMERA" ) )
		EffectSetIsWithCockpit( fxHandle, true )
	}
	else
	{
		return
	}

	OnThreadEnd(
		function() : ( player, fxHandle )
		{
			printf("ALTER - FX Done Portal " + Time())
			if ( fxHandle > -1 )
				EffectStop( fxHandle, true, false )
		}
	)

	
	float maxTime = tuning.warpDelayAfterUse + tuning.warpDelayAfterUseAdditionalWhenKnocked + 2.0
	float endTime = Time() + maxTime

	
	while( Time() < endTime )
	{
		WaitFrame()
		if ( Bleedout_IsBleedingOut( player ) && player.ContextAction_IsActive() )
			break

		if ( player.IsPhaseShifted() )
			break
	}

#if TRANSPORT_PORTAL_DATAPAD_DEBUG
	printf("ALTER - " + player + " - wait done " + Time())
#endif
}




void function TransportPortal_CancelUse( )
{
	entity player = GetLocalViewPlayer()
	if ( !IsValid( player ) )
		return

	entity datapadWeapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
	if ( IsValid( datapadWeapon ) && datapadWeapon.GetWeaponClassName() == TRANSPORT_PORTAL_DATAPAD_WEAPON_NAME )
	{
#if TRANSPORT_PORTAL_DATAPAD_DEBUG
		printf("Alter - " + player + ": TransportPortal_CancelUse - doing cancel")
#endif
		player.ClientCommand( "invnext" )
	}
}

