
global function HopupGunshield_Init
global function HopupGunshield_OnWeaponActivate
global function HopupGunshield_OnWeaponDeactivate
global function HopupGunshield_OnWeaponStartZoomIn
global function HopupGunshield_OnWeaponStartZoomOut


global function ServerToClient_SignalGunshieldShouldStop
global function ServerToClient_CockpitFirstPersonShieldStart
global function ServerToClient_GunshieldBroken


global function IsLGMGunShieldEnt
global function HopupGunshield_ShieldHealth

#if DEV
global function DEV_LMG_Gunshields_ShouldDisable
#endif

global const string HOPUP_GUN_SHIELD_NAME = "hopup_gunshield_scriptname"

const asset LMG_GUN_SHIELD_MODEL = $"mdl/fx/sentry_turret_shield.rmdl"
const asset LMG_GUN_SHIELD_MODEL_SML = $"mdl/fx/sentry_turret_shield_sml.rmdl"
const asset LMG_GUN_SHIELD_MODEL_MED = $"mdl/fx/sentry_turret_shield_med.rmdl"
const asset LMG_GUN_SHIELD_MODEL_LRG = $"mdl/fx/sentry_turret_shield_lrg.rmdl"

const asset LMG_GUN_SHIELD_FX = $"P_sentry_turret_shield"
const asset LMG_GUN_SHIELD_FX_SML = $"P_sentry_turret_shield_sml"
const asset LMG_GUN_SHIELD_FX_MED = $"P_sentry_turret_shield_med"
const asset LMG_GUN_SHIELD_FX_LRG = $"P_sentry_turret_shield_lrg"
const asset GUN_SHIELD_FX_1P = $"P_sentry_turret_shield_FP"

const asset LMG_GUN_SHIELD_BREAK = $"P_sentry_turrent_shield_break_3p"
const asset LMG_GUN_SHIELD_BREAK_FP = $"P_sentry_turret_shield_break_FP"

global const string GUN_SHIELD_3P_SOUND_FRIENDLY = "LMG_GunShield_Sustain_3P_Friendly"
global const string GUN_SHIELD_3P_SOUND_ENEMY = "LMG_GunShield_Sustain_3P_Enemy"
const string GUN_SHIELD_1P_SOUND = "LMG_GunShield_Sustain_1P"
const string GUN_SHIELD_BREAK_1P_SOUND = "LMG_GunShield_Destroyed_1P"
const string GUN_SHIELD_BREAK_3P_SOUND = "LMG_GunShield_Destroyed_3P"

const string GUN_SHIELD_ATTACHPOINT = "PROPGUN"
global const string GUNSHIELD_WEAPON_MOD = "hopup_gunshield"

struct {







		int gunShieldFx

#if DEV
		bool devToggleShieldsOffForEveryone = false
#endif
} file

bool function IsLGMGunShieldEnt( entity ent )
{
	return ent.GetScriptName() == HOPUP_GUN_SHIELD_NAME
}

float function HopupGunshield_ShieldRechargeTime()
{
	return GetCurrentPlaylistVarFloat( "lmg_gunshield_recharge_time", 12.0 )
}

float function HopupGunshield_ShieldHealth()
{
	return GetCurrentPlaylistVarFloat( "lmg_gunshield_shield_health", 40.0 )
}

float function HopupGunshield_ActivationTime()
{
	return GetCurrentPlaylistVarFloat( "lmg_gunshield_shield_activation_time", 0.5 )
}

float function HopupGunshield_Gibraltar_EnergyMax()
{
	return GetCurrentPlaylistVarFloat( "hopup_gunshield_gibby_energymax", 75.0 )
}

void function HopupGunshield_Init()
{
	RegisterSignal( "GunShieldDeactivate" )
	RegisterSignal( "GunShieldBroken" )


		PrecacheParticleSystem( GUN_SHIELD_FX_1P )
		PrecacheParticleSystem( LMG_GUN_SHIELD_BREAK_FP )












	Remote_RegisterClientFunction( "ServerToClient_SignalGunshieldShouldStop", "entity" )
	Remote_RegisterClientFunction( "ServerToClient_CockpitFirstPersonShieldStart", "entity", "entity" )
	Remote_RegisterClientFunction( "ServerToClient_GunshieldBroken", "entity" )






		AddCallback_PlayerClassChanged( OnPlayerClassChanged_ClearGunshield )
		AddOnSpectatorTargetChangedCallback( HopupGunShield_OnSpectatorTargetChanged )

}


























void function HopupGunshield_OnWeaponActivate( entity weapon )
{
	if ( !weapon.HasMod( GUNSHIELD_WEAPON_MOD ) )
		return

	HopupGunshield_Add( weapon )

	
}

void function HopupGunshield_OnWeaponDeactivate( entity weapon )
{
	if ( !weapon.HasMod( GUNSHIELD_WEAPON_MOD ) )
		return

	HopupGunshield_Remove( weapon )
}

void function HopupGunshield_Add( entity weapon )
{
	entity player = weapon.GetWeaponOwner()

	if ( !IsValid( player ) )
		return
























}

void function HopupGunshield_Remove( entity weapon )
{
	entity player = weapon.GetWeaponOwner()

	if ( !IsValid( player ) )
		return






































		Signal( weapon, "GunShieldDeactivate" )

}

void function HopupGunshield_OnWeaponStartZoomIn( entity weapon )
{
	if ( !weapon.HasMod( GUNSHIELD_WEAPON_MOD ) )
		return

	entity weaponOwner = weapon.GetWeaponOwner()

	if ( !IsValid( weaponOwner ) || !IsValid( weapon ) )
		return

	if ( !IsPlayerGibraltar( weaponOwner ) )
	{



	}
}

void function HopupGunshield_OnWeaponStartZoomOut( entity weapon )
{
	if ( !weapon.HasMod( GUNSHIELD_WEAPON_MOD ) )
		return

	entity weaponOwner = weapon.GetWeaponOwner()

	if ( !IsValid( weaponOwner ) )
		return

	if ( !IsPlayerGibraltar( weaponOwner ) )
	{
		Signal( weapon, "GunShieldDeactivate" )

		entity shieldEnt = weapon.GetWeaponUtilityEntity()
		if ( IsValid( shieldEnt ) )
			Signal( shieldEnt, "GunShieldDeactivate" )
	}
}














































bool function IsPlayerGibraltar( entity player )
{
	if ( !IsValid( player ) || !player.IsPlayer() )
		return false

	return PlayerHasPassive( player, ePassives.PAS_ADS_SHIELD )
}

#if DEV
void function DEV_LMG_Gunshields_ShouldDisable( bool toggle )
{
	
	if ( toggle == true )
	{
		file.devToggleShieldsOffForEveryone = true
	}
	if ( toggle == false )
	{
		file.devToggleShieldsOffForEveryone = false
	}
}
#endif












































































































































































































































































































































































































































































void function HopupGunshield_FirstPersonShield( entity player, entity weapon, asset shieldEffect, string attachment )
{
#if DEV
		if ( file.devToggleShieldsOffForEveryone )
			return
#endif

	EndSignal( player, "OnDeath", "OnDestroy", "BleedOut_OnStartDying" )
	EndSignal( weapon, "OnDestroy", "GunShieldDeactivate" )

	wait HopupGunshield_ActivationTime()

	if ( !IsValid( weapon ) || !IsValid( shieldEffect ) )
		return

	TrackFirstPersonGunShield( weapon, shieldEffect, attachment )
}

void function ServerToClient_SignalGunshieldShouldStop( entity weapon )
{
	if ( !IsValid( weapon ) )
		return

	entity weaponOwner = weapon.GetWeaponOwner()
	if ( !IsValid( weaponOwner ) )
		return

	if ( !IsPlayerGibraltar( weaponOwner ) )
	{
		Signal( weapon, "GunShieldDeactivate" )
	}
}

void function ServerToClient_CockpitFirstPersonShieldStart( entity player, entity weapon )
{
	if ( !IsValid( player ) || !IsValid( weapon ) )
		return

	thread HopupGunshield_CockpitFirstPersonShield( player, weapon )
}

void function HopupGunshield_CockpitFirstPersonShield( entity player, entity weapon )
{
	if ( !IsValid( player ) || !IsValid( weapon ) )
		return

	EndSignal( player, "OnDeath", "OnDestroy", "BleedOut_OnStartDying", "GunShieldBroken" )
	EndSignal( weapon, "OnDestroy", "GunShieldDeactivate" )

	entity shieldEnt = weapon.GetWeaponUtilityEntity()
	if ( !IsValid( shieldEnt ) )
		return

	entity cockpit = player.GetCockpit()
	if ( !IsValid( cockpit ) )
		return

	int fxID   = GetParticleSystemIndex( GUN_SHIELD_FX_1P )
	file.gunShieldFx = StartParticleEffectOnEntity( cockpit, fxID, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetIsWithCockpit( file.gunShieldFx, true )
	EffectSetControlPointVector( file.gunShieldFx, 1, TEAM_COLOR_FRIENDLY )

	OnThreadEnd(
		function() : ()
		{
			if ( EffectDoesExist( file.gunShieldFx ) )
			{
				EffectStop( file.gunShieldFx, true, true )
			}
		}
	)

	while ( PlayerIsInADS( player ) )
	{
		if ( !IsValid( weapon ) )
			return

		entity gunshield = weapon.GetWeaponUtilityEntity()

		if ( IsValid( gunshield ) )
		{
			float frac = gunshield.GetHealth() / HopupGunshield_ShieldHealth()
			vector colorVec = GetTriLerpColor( frac, COLOR_WHITE, LerpVector( TEAM_COLOR_FRIENDLY, <255, 255, 255>, 0.5 ), TEAM_COLOR_FRIENDLY, 0.6, 0.3 )
			if ( EffectDoesExist( file.gunShieldFx ) )
				EffectSetControlPointVector( file.gunShieldFx, 1, colorVec )
		}

		WaitFrame()
	}
}

void function ServerToClient_GunshieldBroken( entity player )
{
	if ( !IsValid( player ) )
		return

	Signal( player, "GunShieldBroken" )
	thread HopupGunshield_CockpitFirstPersonShield_Break( player )
}

void function HopupGunshield_CockpitFirstPersonShield_Break( entity player )
{
	if ( !IsValid( player ) )
		return

	entity viewPlayer = GetLocalViewPlayer()
	if ( player != viewPlayer )
		return

	int shieldBreakFx = StartParticleEffectOnEntity( viewPlayer, GetParticleSystemIndex( LMG_GUN_SHIELD_BREAK_FP ), FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetIsWithCockpit( shieldBreakFx, true )

	OnThreadEnd(
		function() : ( shieldBreakFx )
		{
			if ( EffectDoesExist( shieldBreakFx ) )
				CleanupFXHandle( shieldBreakFx )
		}
	)

	wait 1.0

	return
}

bool function HopupGunshield_ShouldShowGunshield( entity player )
{
	if ( !IsValid( player ) )
		return false

	if ( player.IsPlayer() == false )
		return false

	if ( IsPlayerGibraltar( player ) )
		return false

	entity weapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
	if ( !IsValid( weapon ) )
		return false

	if ( weapon.HasMod( GUNSHIELD_WEAPON_MOD ) == false )
		return false

	if ( PlayerIsInADS( player ) == false )
		return false

	entity shieldEnt = weapon.GetWeaponUtilityEntity()

	if ( !IsValid( shieldEnt ) )
		return false

	if ( shieldEnt.GetHealth() <= 0 )
		return false

	return true
}

void function OnPlayerClassChanged_ClearGunshield( entity player )
{
	
	
	if ( HopupGunshield_ShouldShowGunshield( player ) )
		return

	entity weapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
	if ( !IsValid( weapon ) )
		return

	Signal( weapon, "GunShieldDeactivate" )
}

void function HopupGunShield_OnSpectatorTargetChanged( entity observer, entity prevTarget, entity newTarget )
{
	if ( HopupGunshield_ShouldShowGunshield( newTarget ) )
	{
		entity weapon = newTarget.GetActiveWeapon( eActiveInventorySlot.mainHand )
		if ( !IsValid( weapon ) )
			return

		thread HopupGunshield_CockpitFirstPersonShield( newTarget, weapon )
	}	
}

      