
global function OnWeaponActivate_ability_shadow_form
global function OnWeaponTossReleaseAnimEvent_ability_shadow_form
global function OnWeaponAttemptOffhandSwitch_ability_shadow_form
global function OnWeaponOwnerChanged_ability_shadow_form
global function MpAbilityShadowForm_Init
global function IsInForgedShadows
global function IsForgedShadowsShield
global function ShadowShield_IsAllowedStickyEnt






global function ServerToClient_UpdateDamageRUI
global function ServerToClient_DoShadowShieldDamageIndicator

global function MpAbilityShadowForm_GetEnableHealFlash
global function MpAbilityShadowForm_IsRecharging
global function MpAbilityShadowForm_PopulateRui


const string ABILITY_USED_MOD = "ability_used_mod"

global const string FORGED_SHADOWS_SHIELD_NAME = "forged_shadows_shield"


const string SHADOW_FORM_ACTIVATE_SOUND_1P = "DeathProtection_Reborn_Activate_1P"
const string SHADOW_FORM_LOOP_SOUND_1P = "DeathProtection_Reborn_Loop_1P"
const string SHADOW_FORM_DEACTIVATE_SOUND_1P = "DeathProtection_Reborn_End_1P"
const string SHADOW_FORM_WINDUP_SOUND_1P = "DeathProtection_Reborn_Windup_1P"
const string SHADOW_FORM_EXPIRATION_WARNING_1P = "DeathProtection_Reborn_WarningToEnd_1p"
const string SHADOW_FORM_COUNTDOWN_SOUND = "TDM_UI_InGame_Countdown"
const string SHADOW_FORM_SHIELD_BREAK_SOUND_1P = "DeathProtection_Reborn_Shield_Broken_1P"
const string SHADOW_FORM_SHIELD_BREAKER_SOUND_1P = "DeathProtection_Reborn_Shield_Broken_1P_Enemy"
const string SHADOW_FORM_SHIELD_RECHARGE_SOUND_1P = "DeathProtection_Reborn_Shield_Recharge_1P"
const string SHADOW_FORM_SHIELD_CHARGED_SOUND_1P = "DeathProtection_Reborn_Shield_Charged_1P"
const string SHADOW_FORM_SHIELD_HIT_SOUND_1P = "DeathProtection_Reborn_Shield_Damage_1P"

const string SHADOW_FORM_ACTIVATE_SOUND_3P = "DeathProtection_Reborn_Activate_3P"
const string SHADOW_FORM_LOOP_SOUND_3P = "DeathProtection_Reborn_Loop_3P"
const string SHADOW_FORM_DEACTIVATE_SOUND_3P = "DeathProtection_Reborn_End_3P"
const string SHADOW_FORM_WINDUP_SOUND_3P = "DeathProtection_Reborn_Windup_3P"
const string SHADOW_FORM_EXPIRATION_WARNING_3P = "DeathProtection_Reborn_WarningToEnd_3p"
const string SHADOW_FORM_SHIELD_BREAK_SOUND_3P = "DeathProtection_Reborn_Shield_Broken_3P"
const string SHADOW_FORM_SHIELD_RECHARGE_SOUND_3P = "DeathProtection_Reborn_Shield_Recharge_3P"
const string SHADOW_FORM_SHIELD_CHARGED_SOUND_3P = "DeathProtection_Reborn_Shield_Charged_3P"
const string SHADOW_FORM_SHIELD_HIT_SOUND_3P = "DeathProtection_Reborn_Shield_Damage_3P"



const asset SHADOW_FORM_SHIELD_HIT_FX_1P = $"P_armor_FP_Hit_rev"
const asset SHADOW_FORM_SHIELD_HIT_FX2_1P = $"P_health_hex_rev_fast"
const asset SHADOW_FORM_SHIELD_ACTIVE_FX_1P = $"p_rev_reborn_FP_ult_screen_shield"


const asset SHADOW_FORM_ACTIVATION_FX_3P = $"p_rev_reborn_3P_ult_activation_arms"
const asset SHADOW_FORM_SHIELD_ACTIVATION_FX_3P = $"P_rev_reborn_shield_start"
const asset SHADOW_FORM_SHIELD_FX_3P = $"P_rev_reborn_shield"
const asset SHADOW_FORM_SHIELD_RECHARGE_FX_3P = $"P_rev_reborn_3P_ult_activation"
const asset SHADOW_FORM_SHIELD_BREAK_FX_3P = $"P_rev_reborn_shield_end"
const asset SHADOW_FORM_BODY_FX_3P = $"P_rev_reborn_shield_powerup"
const asset SHADOW_FORM_EYE_FX_3P = $"P_rev_reborn_shield_eye"
const asset SHADOW_FORM_TRAIL_FX_3P = $"P_rev_reborn_3P_ult_body_trail"
const asset SHADOW_FORM_SHIELD_HIT_FX_3P = $"P_rev_reborn_shield_dmg"


const asset SHADOW_FORM_SHIELD_COLLISION_MODEL = $"mdl/fx/forged_shadows_shield_physX.rmdl"



const asset SHADOW_FORM_SHADOW_SCREEN_FX = $"p_rev_reborn_FP_ult_screen_base"




global const float SHADOW_FORM_DURATION = 25.0
const float SHADOW_FORM_TIME_ADD = 5.0
const float SHADOW_FORM_ASSIST_TIME = 3.0
const float SHADOW_FORM_SHIELD_HEALTH = 75
const float SHADOW_FORM_SHIELD_REGEN_RATE = 1.0
const float SHADOW_FORM_SHIELD_REGEN_DELAY = 1.0
const float SHADOW_FORM_EXPIRATION_WARNING_TIME = 3.0
const vector SHADOW_FORM_SHIELD_ORIGIN_OFFSET= < 0, 0, -5 >
const int SHADOW_FORM_ENABLE_SHIELD_RESET = 1
const int SHADOW_FORM_ENABLE_TAC_RESET = 1
const int SHADOW_FORM_ENABLE_CARRYOVER_DMG = 1

const int SHADOW_FORM_CHEST_FOCUS = 1

global const string SHADOW_FORM_HEALTH_NETVAR = "revenant_shadow_health"

struct
{








		int colorCorrection
		int shadowShieldFx

		bool isRecharging 	  = false
		bool enableHealFlash  = false
		float lastKillTime    = -1

		bool enableDmgFlash   = false

		float mitigatedDamageRemaining = 0.0


	
	float duration
	float extensionTime
	float assistTime
	float shieldHealth
	float shieldRegenRate
	float shieldRegenDelay
	bool enableShieldReset
	bool enableTacReset
	bool enableCarryoverDmg
} file

void function MpAbilityShadowForm_Init()
{
	
	PrecacheParticleSystem( SHADOW_FORM_SHIELD_HIT_FX_1P )
	PrecacheParticleSystem( SHADOW_FORM_SHIELD_HIT_FX2_1P )

	
	PrecacheParticleSystem( SHADOW_FORM_ACTIVATION_FX_3P )
	PrecacheParticleSystem( SHADOW_FORM_SHIELD_ACTIVATION_FX_3P )
	PrecacheParticleSystem( SHADOW_FORM_SHIELD_FX_3P )
	PrecacheParticleSystem( SHADOW_FORM_SHIELD_RECHARGE_FX_3P )
	PrecacheParticleSystem( SHADOW_FORM_SHIELD_BREAK_FX_3P )
	PrecacheParticleSystem( SHADOW_FORM_BODY_FX_3P )
	PrecacheParticleSystem( SHADOW_FORM_EYE_FX_3P )
	PrecacheParticleSystem( SHADOW_FORM_TRAIL_FX_3P )
	PrecacheParticleSystem( SHADOW_FORM_SHIELD_HIT_FX_3P )

	
	PrecacheModel( SHADOW_FORM_SHIELD_COLLISION_MODEL )

	
	file.duration = GetCurrentPlaylistVarFloat( "revenant_shadow_form_duration", SHADOW_FORM_DURATION )
	file.extensionTime = GetCurrentPlaylistVarFloat( "revenant_shadow_form_extension_time", SHADOW_FORM_TIME_ADD )
	file.assistTime = GetCurrentPlaylistVarFloat( "revenant_shadow_form_extension_time", SHADOW_FORM_ASSIST_TIME )
	file.shieldHealth = GetCurrentPlaylistVarFloat( "revenant_shadow_form_shield_health", SHADOW_FORM_SHIELD_HEALTH )
	file.shieldRegenRate = GetCurrentPlaylistVarFloat( "revenant_shadow_form_shield_regen_rate", SHADOW_FORM_SHIELD_REGEN_RATE )
	file.shieldRegenDelay = GetCurrentPlaylistVarFloat( "revenant_shadow_form_shield_regen_delay", SHADOW_FORM_SHIELD_REGEN_DELAY )
	file.enableShieldReset = ( GetCurrentPlaylistVarInt( "revenant_shadow_form_enable_shield_reset", SHADOW_FORM_ENABLE_SHIELD_RESET ) > 0 )
	file.enableTacReset = ( GetCurrentPlaylistVarInt( "revenant_shadow_form_enable_tactical_reset", SHADOW_FORM_ENABLE_TAC_RESET ) > 0 )
	file.enableCarryoverDmg = ( GetCurrentPlaylistVarInt( "revenant_shadow_form_enable_carryover_dmg", SHADOW_FORM_ENABLE_CARRYOVER_DMG ) > 0 )

	RegisterNetworkedVariable( SHADOW_FORM_HEALTH_NETVAR, SNDC_PLAYER_GLOBAL, SNVT_FLOAT_RANGE, 0.0, 0.0, file.shieldHealth )
	Remote_RegisterClientFunction( "ServerToClient_UpdateDamageRUI", "bool", "bool", "float", 0.0, 32000.0, 32  )
	Remote_RegisterClientFunction( "ServerToClient_DoShadowShieldDamageIndicator", "entity", "entity", "int", INT_MIN, INT_MAX )









		PrecacheParticleSystem( SHADOW_FORM_SHADOW_SCREEN_FX )
		PrecacheParticleSystem( SHADOW_FORM_SHIELD_ACTIVE_FX_1P )
		RegisterSignal( "ShadowForm_EndShadowScreenFx" )
		RegisterSignal( "EndDamageFlash" )
		RegisterSignal( "EndHealFlash" )
		AddCallback_OnBleedoutStarted( ShadowForm_OnBleedoutStarted_Client )
		file.colorCorrection = ColorCorrection_Register( "materials/correction/ability_silence.raw_hdr" )
		StatusEffect_RegisterEnabledCallback( eStatusEffect.shadow_form, ShadowForm_StartClient )
		StatusEffect_RegisterDisabledCallback( eStatusEffect.shadow_form, ShadowForm_StopClient )


	RegisterSignal( "ExitShadowForm" )
	RegisterSignal( "EndShieldRegenDelay" )
	RegisterSignal( "EndExpirationThread" )
}





































void function ShadowForm_OnBleedoutStarted_Client( entity player, float endTime )
{
	if ( player == GetLocalViewPlayer() )
	{
		if ( IsInForgedShadows( player ) )
		{
			player.Signal( "ExitShadowForm" )
		}
	}
}


bool function OnWeaponAttemptOffhandSwitch_ability_shadow_form( entity weapon )
{
	entity weaponOwner = weapon.GetOwner()

	if ( !IsValid( weaponOwner ) )
		return false

	if( IsInForgedShadows( weaponOwner ) )
		return false

	return true
}

void function OnWeaponActivate_ability_shadow_form( entity weapon )
{
	entity player = weapon.GetOwner()
	if( !IsValid( player ) )
		return










}

var function OnWeaponTossReleaseAnimEvent_ability_shadow_form( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	entity ownerPlayer = weapon.GetOwner()
	Assert( ownerPlayer.IsPlayer() )

	thread ShadowForm_Start( ownerPlayer, weapon )

	PlayerUsedOffhand( ownerPlayer, weapon )

	return weapon.GetAmmoPerShot()
}

void function OnWeaponOwnerChanged_ability_shadow_form( entity weapon, WeaponOwnerChangedParams changeParams )
{
	entity oldOwner = changeParams.oldOwner
	if( IsValid( oldOwner ) )
		oldOwner.Signal( "ExitShadowForm" )
}

bool function IsInForgedShadows( entity player )
{
	if ( !IsValid( player ) )
		return false

	if ( StatusEffect_HasSeverity( player, eStatusEffect.shadow_form ) )
		return true

	return false
}

bool function IsForgedShadowsShield( entity ent )
{
	if( !IsValid( ent ) )
		return false

	return ent.GetTargetName() == FORGED_SHADOWS_SHIELD_NAME
}




















bool function ShadowShield_IsAllowedStickyEnt( entity shadowShield, entity stickyEnt, string stickyEntWeaponClassName )
{
	if( !IsValid( shadowShield ) || !IsValid( stickyEnt ) )
		return false
	if( IsFriendlyTeam( stickyEnt.GetTeam(), shadowShield.GetTeam() ) )
		return false

	bool allowStick = false

	if ( stickyEntWeaponClassName == "mp_weapon_cluster_bomb_launcher" )
		allowStick = true

	if ( stickyEntWeaponClassName == "mp_weapon_arc_bolt" )
		allowStick = true

	if ( stickyEntWeaponClassName == GRENADE_EMP_WEAPON_NAME )
		allowStick = true

	if( allowStick )
		thread ShadowShield_TrackStickyEnt_Thread( shadowShield, stickyEnt )

	return allowStick
}


void function ShadowShield_TrackStickyEnt_Thread( entity shadowShield, entity stickyEnt )
{
	EndSignal( shadowShield, "OnDestroy" )
	EndSignal( stickyEnt, "OnDestroy" )

	bool hadLoS = true

	array<entity> ignoreArray	= ShadowShieldIgnoreArray()
	TraceResults initialTrace = TraceLine( shadowShield.GetOrigin(), stickyEnt.GetOrigin(), ignoreArray, TRACE_MASK_VISIBLE, TRACE_COLLISION_GROUP_NONE )

	if(initialTrace.fraction < 1)
		hadLoS = false

	WaitFrame() 

	while ( true )
	{
		if( !IsValid( shadowShield ) )
			return
		if( !IsValid( stickyEnt ) )
			return

		ignoreArray	= ShadowShieldIgnoreArray()
		TraceResults results = TraceLine( shadowShield.GetOrigin(), stickyEnt.GetOrigin(), ignoreArray, TRACE_MASK_VISIBLE, TRACE_COLLISION_GROUP_NONE )
		if(	results.fraction < 1 )
		{





			return
		}

		WaitFrame()
	}
}

array<entity> function ShadowShieldIgnoreArray()
{
	array<entity> ignoreArray = GetPlayerArray_Alive()

	array<entity> shadowShields = GetEntArrayByScriptName( FORGED_SHADOWS_SHIELD_NAME ) 
	foreach ( shadowShield in shadowShields )
	{
		if( !IsValid( shadowShield ) )
			continue
		ignoreArray.append( shadowShield )
	}

	array<entity> mobileShields = GetEntArrayByScriptName( MOBILE_SHIELD_SCRIPTNAME ) 
	foreach ( shieldWall in mobileShields )
	{
		if( !IsValid(shieldWall) )
			continue
		ignoreArray.append( shieldWall )
	}

	array<entity> thrownShields = GetEntArrayByScriptName( SHIELD_THROW_SCRIPTNAME ) 
	foreach ( shield in thrownShields )
	{
		if( !IsValid(shield) )
			continue
		ignoreArray.append( shield )
	}

	array<entity> bubbleShields = GetEntArrayByScriptName( BUBBLE_SHIELD_SCRIPTNAME ) 
	foreach ( bubble in bubbleShields )
	{
		if( !IsValid(bubble) )
			continue
		ignoreArray.append( bubble )
	}

	array<entity> holoEnts = GetPlayerDecoyArray() 
	ignoreArray.extend( holoEnts )

	return ignoreArray
}

void function ShadowForm_Start( entity player, entity weapon )
{
	Assert ( IsNewThread(), "Must be threaded off" )
	if ( !IsAlive( player ) )
		return

	EndSignal( player, "OnDeath", "OnDestroy", "BleedOut_OnStartDying" ,"ExitShadowForm" )

























































































































































	while( StatusEffect_HasSeverity( player, eStatusEffect.shadow_form ) )
	{






		WaitFrame()
	}
}










































































































































































































































































































































































































































































































































































































void function ShadowForm_StartClient( entity ent, int statusEffect, bool actuallyChanged )
{
	if ( !actuallyChanged && GetLocalViewPlayer() == GetLocalClientPlayer() )
		return

	if ( ent != GetLocalViewPlayer() )
		return

	thread ShadowForm_FXThink( ent )
}

void function ShadowForm_StopClient( entity ent, int statusEffect, bool actuallyChanged )
{
	if ( !actuallyChanged && GetLocalViewPlayer() == GetLocalClientPlayer() )
		return

	if ( ent != GetLocalViewPlayer() )
		return

	Rumble_Play( "rumble_burn_card_activate", {} )

	ent.Signal( "ShadowForm_EndShadowScreenFx" )
}

void function ShadowForm_FXThink( entity player )
{
	if( !IsValid( player ) )
		return
	entity cockpit = player.GetCockpit()
	if( !IsValid( cockpit ) )
		return

	EndSignal( player, "OnDeath", "OnDestroy", "BleedOut_OnStartDying", "ShadowForm_EndShadowScreenFx" )

	int fxHandle = StartParticleEffectOnEntity( cockpit, GetParticleSystemIndex( SHADOW_FORM_SHADOW_SCREEN_FX ), FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetIsWithCockpit( fxHandle, true )
	vector controlPoint = <1, 1, 1>
	EffectSetControlPointVector( fxHandle, 1, controlPoint )

	thread ColorCorrection_LerpWeight( file.colorCorrection, 0, 1, 0.25 )

	thread ShadowShield_ToggleFX_Think( player )

	EmitSoundOnEntity( player, SHADOW_FORM_ACTIVATE_SOUND_1P ) 
	EmitSoundOnEntity( player, SHADOW_FORM_LOOP_SOUND_1P )  

	file.mitigatedDamageRemaining = file.shieldHealth

	OnThreadEnd(
		function() : ( player, fxHandle )
		{
			if( IsValid( player ) )
			{
				StopSoundOnEntity( player, SHADOW_FORM_LOOP_SOUND_1P ) 
				EmitSoundOnEntity( player, SHADOW_FORM_DEACTIVATE_SOUND_1P ) 
			}

			if ( EffectDoesExist( fxHandle ) )
				EffectStop( fxHandle, true, true )

			thread ColorCorrection_LerpWeight( file.colorCorrection, 1, 0, 1 )
		}
	)

	WaitForever()
}

void function ShadowShield_ToggleFX_Think( entity player )
{
	if( !IsValid( player ) )
		return
	entity cockpit = player.GetCockpit()
	if( !IsValid( cockpit ) )
		return
	entity viewPlayer = GetLocalViewPlayer()
	if ( player != GetLocalViewPlayer() )
		return

	EndSignal( player, "OnDeath", "OnDestroy", "BleedOut_OnStartDying", "ShadowForm_EndShadowScreenFx" )

	file.shadowShieldFx = StartParticleEffectOnEntity( viewPlayer, GetParticleSystemIndex( SHADOW_FORM_SHIELD_ACTIVE_FX_1P ), FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetIsWithCockpit( file.shadowShieldFx, true )
	Rumble_Play( "rumble_stim_activate", {} )

	OnThreadEnd(
		function() : ()
		{
			if ( EffectDoesExist( file.shadowShieldFx ) )
				EffectStop( file.shadowShieldFx, true, true )
		}
	)

	bool effectiveActive = true
	while( true )
	{
		float shieldHealth = player.GetPlayerNetFloat( SHADOW_FORM_HEALTH_NETVAR )
		if( effectiveActive && ( shieldHealth <= 0.0 ) )
		{
			if( EffectDoesExist( file.shadowShieldFx ) )
				EffectStop( file.shadowShieldFx, true, true )
			effectiveActive = false
		}
		if( !effectiveActive && ( shieldHealth == file.shieldHealth ) )
		{
			file.shadowShieldFx = StartParticleEffectOnEntity( viewPlayer, GetParticleSystemIndex( SHADOW_FORM_SHIELD_ACTIVE_FX_1P ), FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
			EffectSetIsWithCockpit( file.shadowShieldFx, true )
			effectiveActive = true
			Rumble_Play( "rumble_stim_activate", {} )
		}

		WaitFrame()
	}
}


void function MpAbilityShadowForm_PopulateRui( entity player,  var rui )
{
	if( rui != null )
	{
		RuiTrackFloat( rui, "timeRemaining", player, RUI_TRACK_STATUS_EFFECT_TIME_REMAINING, eStatusEffect.shadow_form )
		RuiSetFloat( rui, "mitigatedDamageRemaining", file.mitigatedDamageRemaining )
		RuiSetFloat( rui, "maxMitigatedDamage", file.shieldHealth )
		RuiSetBool( rui, "enableDmgFlash", file.enableDmgFlash )
		RuiSetBool( rui, "enableHealFlash", file.enableHealFlash )
		RuiSetBool( rui, "isRecharging", file.isRecharging )
		RuiSetGameTime( rui, "lastKillTime", file.lastKillTime )
	}
}



bool function MpAbilityShadowForm_GetEnableHealFlash()
{
	return file.enableHealFlash
}



bool function MpAbilityShadowForm_IsRecharging()
{
	return file.isRecharging
}


void function ServerToClient_DoShadowShieldDamageIndicator( entity player, entity attacker, int damageSourceId )
{
	if ( IsValid( player ) && IsValid( attacker ) )
	{
		if ( player == GetLocalViewPlayer() )
		{
			vector damageOrigin = attacker.GetWorldSpaceCenter()
			DamageIndicators( damageOrigin, attacker, damageSourceId )
		}
	}
}

void function ServerToClient_UpdateDamageRUI( bool isHeal, bool isKill , float shadowFormHealth )
{
	entity player = GetLocalViewPlayer()

	if( IsInForgedShadows( player ) )
	{
		file.mitigatedDamageRemaining = shadowFormHealth

		if( isHeal )
		{
			if( !isKill )
				thread HealFlash( player )
			else
				file.lastKillTime = Time()
		}
		else
			thread DamageTakenFlash( player )
	}
}

void function DamageTakenFlash( entity player )
{
	if( !IsValid( player ) )
		return
	entity viewPlayer = GetLocalViewPlayer()
	if( player != viewPlayer )
		return

	player.Signal( "EndDamageFlash" )
	EndSignal( player, "EndDamageFlash" )

	int shieldDamageFx = StartParticleEffectOnEntity( viewPlayer, GetParticleSystemIndex( SHADOW_FORM_SHIELD_HIT_FX_1P ), FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetIsWithCockpit( shieldDamageFx, true )
	int shieldDamageHexFx = StartParticleEffectOnEntity( viewPlayer, GetParticleSystemIndex( SHADOW_FORM_SHIELD_HIT_FX2_1P ), FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetIsWithCockpit( shieldDamageHexFx, true )

	Rumble_Play( "rumble_pilot_hurt", {} )

	OnThreadEnd(
		function() : ( player, shieldDamageFx, shieldDamageHexFx )
		{
			if( IsAlive( player ) )
				file.enableDmgFlash = true

			if( EffectDoesExist( shieldDamageFx ) )
				EffectStop( shieldDamageFx, true, true )

			if( EffectDoesExist( shieldDamageHexFx ) )
				EffectStop( shieldDamageHexFx, true, true )
		}
	)

	file.enableDmgFlash = false

	wait 0.2

	return
}

void function HealFlash( entity player )
{
	player.Signal( "EndHealFlash" )
	EndSignal( player, "EndHealFlash" )

	OnThreadEnd(
		function() : ( player )
		{
			file.isRecharging    = false
			file.enableHealFlash = false
		}
	)

	file.isRecharging    = true
	file.enableHealFlash = true

	wait 0.4

	return
}

void function ColorCorrection_LerpWeight( int colorCorrection, float startWeight, float endWeight, float lerpTime = 0 )
{
	float startTime = Time()
	float endTime = startTime + lerpTime
	ColorCorrection_SetExclusive( colorCorrection, true )

	while ( Time() <= endTime )
	{
		WaitFrame()
		float weight = GraphCapped( Time(), startTime, endTime, startWeight, endWeight )
		ColorCorrection_SetWeight( colorCorrection, weight )
	}

	ColorCorrection_SetWeight( colorCorrection, endWeight )
}

                            