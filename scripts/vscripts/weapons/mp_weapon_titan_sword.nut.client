


















global function MpWeaponTitanSword_Init
global function OnWeaponActivate_weapon_titan_sword
global function OnWeaponDeactivate_weapon_titan_sword
global function OnMeleeWeaponPrimaryAttack_weapon_titan_sword
global function OnMeleeWeaponSecondaryAttack_weapon_titan_sword
global function OnWeaponCustomActivityEnd_weapon_titan_sword
global function OnWeaponPrimaryAttackAnimEvent_weapon_titan_sword


global function TitanSword_GetMainWeapon
global function TitanSword_ActiveWeaponIsTitanSword
global function TitanSword_WeaponIsTitanSword
global function TitanSword_WeaponRefIsTitanSword
global function TitanSword_DamageSourceIsTitanSword

global function TitanSword_PostCopySanityCheck

global function TitanSword_TryUseFuel
global function TitanSword_HasFuel
global function TitanSword_FillFuel

global function TitanSword_VictimHitOverride
global function TitanSword_LaunchEntity

global function TitanSword_SafelyAddAttackMod
global function TitanSword_CanGoThroughBlockingEntity









global function TitanSword_DisplayHint
global function TitanSword_GetFuelRui
global function TitanSword_ClientPredictCheck


#if DEV





#endif


global const string TITAN_SWORD_WEAPON_REF = "mp_weapon_titan_sword"
const string TITAN_SWORD_LIGHT_SUPER_MOD = "super_melee"

const string TITAN_SWORD_LOOT_MOVER_SCRIPTNAME = "titan_sword_loot_mover"


const string PVAR_TITAN_SWORD_NITRO_TEST = "titan_sword_nitro_test"


const string SIG_TITAN_SWORD_STOP_LOOT_ANIM = "TitanSword_StopLootAnim"
global const string SIG_TITAN_SWORD_DEACTIVATE = "TitanSword_Deactivate"
const string SIG_TITAN_SWORD_DESTROY_FUEL_RUI = "TitanSword_DestroyFuelRui"


const int TITAN_SWORD_OFFHAND_SLOT = OFFHAND_GENERIC

const float TITAN_SWORD_DASH_NOT_READY_DEBOUNCE_TIME_SEC = 1
const float TITAN_SWORD_MAIN_INSTRUCTIONS_DEBOUNCE_TIME = 45 
const float TITAN_SWORD_INSTRUCTIONS_DEBOUNCE_TIME = 10


const asset VFX_TITAN_SWORD_SPEED_TRAIL_BODY = $"P_pilot_dash_trail"
const string VFX_TITAN_SWORD_IMPACT = "pilot_sword_solo"    

const asset FX_TITAN_SWORD_LIGHT_SWIPE_FP = $"P_pilot_sword_swipe_light_FP"
const asset FX_TITAN_SWORD_LIGHT_SWIPE_3P = $"P_pilot_sword_swipe_light_3P"


const string SFX_TITAN_SWORD_FUEL_READY = "titansword_special_ready_1p"
const string SFX_TITAN_SWORD_FUEL_NOT_READY = "titansword_special_notready_1p"
const string SFX_TITAN_SWORD_LOOT_AIR_LOOP = "titansword_drop_air_spin_LP_3P"


const asset RUI_TITAN_SWORD_FUEL_HUD = $"ui/titan_sword_dash_meter.rpak"


const int GOING_THROUGH_FALSE = 0
const int GOING_THROUGH_TRUE = 1
const int GOING_THROUGH_FORCE_HIT = 2

struct
{

		float debounceMainMsg
		float debounceInstructionMsg
		float debounceDashMsg = -1
		var   fuelRui





}file

bool function TestNitro()
{
	return GetCurrentPlaylistVarBool( PVAR_TITAN_SWORD_NITRO_TEST, false )
}

bool function TitanSword_PostCopySanityCheck( string id )
{
	return GetCurrentPlaylistVarBool( "titan_sword_post_copy_" + id, true )
}































void function MpWeaponTitanSword_Init()
{
	PrecacheWeapon( TITAN_SWORD_WEAPON_REF )

	PrecacheImpactEffectTable( VFX_TITAN_SWORD_IMPACT )

	PrecacheParticleSystem( VFX_TITAN_SWORD_SPEED_TRAIL_BODY )

	PrecacheParticleSystem( FX_TITAN_SWORD_LIGHT_SWIPE_FP )
	PrecacheParticleSystem( FX_TITAN_SWORD_LIGHT_SWIPE_3P )

	RegisterSignal( SIG_TITAN_SWORD_DEACTIVATE )

	
	MpWeaponTitanSword_Block_Init()
	MpWeaponTitanSword_Dash_Init()
	MpWeaponTitanSword_Heavy_Init()
	MpWeaponTitanSword_Launcher_Init()
	MpWeaponTitanSword_Slam_Init()
	MpWeaponTitanSword_Super_Init()
	MpWeaponTitanSword_Light_Init()


		StatusEffect_RegisterDisabledCallback( eStatusEffect.titan_sword_fuel, TitanSword_OnFuelFull )
		AddCallback_OnPrimaryWeaponStatusUpdate( OnPrimaryWeaponStatusUpdate_TitanSword )












	RegisterSignal( SIG_TITAN_SWORD_DESTROY_FUEL_RUI )
}

void function MpWeaponTitanSword_Light_Init( )
{
	
}

void function TitanSword_Light_StartVFX( entity weapon )
{
	

	entity player = weapon.GetWeaponOwner()
	{
	weapon.PlayWeaponEffect( FX_TITAN_SWORD_LIGHT_SWIPE_FP, FX_TITAN_SWORD_LIGHT_SWIPE_3P, "blade_tip" )
	}
}

void function TitanSword_Light_StopVFX( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	
	{
	weapon.StopWeaponEffect( FX_TITAN_SWORD_LIGHT_SWIPE_FP, FX_TITAN_SWORD_LIGHT_SWIPE_3P )
	}
}

void function OnWeaponActivate_weapon_titan_sword( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	if ( !IsValid( player ) )
		return

	TitanSword_Block_OnWeaponActivate( player, weapon )
	TitanSword_Dash_OnWeaponActivate ( player, weapon )
	TitanSword_Heavy_OnWeaponActivate ( player, weapon )
	TitanSword_Launcher_OnWeaponActivate ( player, weapon )
	TitanSword_Slam_OnWeaponActivate( player, weapon )
	TitanSword_Super_OnWeaponActivate( player, weapon )

	TitanSword_UpdateFuelCrosshair( player, weapon )






		TitanSword_DisplayHint( player, "#WPN_TITAN_SWORD_HINT", 5.0, TITAN_SWORD_MAIN_INSTRUCTIONS_DEBOUNCE_TIME )
		thread TitanSword_FuelMeterRui_Thread( weapon, player )

}

void function OnWeaponDeactivate_weapon_titan_sword( entity weapon )
{
	entity player = weapon.GetWeaponOwner()
	if ( !IsValid( player ) )
		return

	TitanSword_Dash_OnWeaponDeactivate( player, weapon )
	TitanSword_Slam_OnWeaponDectivate( player, weapon )
	TitanSword_Super_OnWeaponDeactivate( player, weapon )
	TitanSword_ClearMods( weapon )
	weapon.Signal( SIG_TITAN_SWORD_DEACTIVATE )
}

int function OnMeleeWeaponPrimaryAttack_weapon_titan_sword( entity weapon, entity player )
{
	

	
	if ( TitanSword_Dash_TryDash( player, weapon ) )
		return PLAYER_MELEE_STATE_NONE

	
	if ( TitanSword_Slam_TrySlam( player, weapon ) )
		return PLAYER_MELEE_STATE_SLAM_ATTACK

	
	TitanSword_Heavy_TryHeavyAttack( player, weapon )
	return PLAYER_MELEE_STATE_KICK_ATTACK
}

int function OnMeleeWeaponSecondaryAttack_weapon_titan_sword( entity weapon, entity player )
{
	if ( !weapon.IsReadyToFire() )
		return PLAYER_MELEE_STATE_NONE

	
	if ( TitanSword_Launcher_TryLauncher( player, weapon ) )
		return PLAYER_MELEE_STATE_KICK_ATTACK

	
	if ( TitanSword_Super_IsActive( player ) )
		weapon.AddMod( TITAN_SWORD_LIGHT_SUPER_MOD )

	
	TitanSword_Light_StartVFX ( weapon )
	return PLAYER_MELEE_STATE_KICK_ATTACK
}

void function OnWeaponCustomActivityEnd_weapon_titan_sword( entity weapon, int activity )
{
	
	switch( activity )
	{
		case ACT_VM_MELEE_ATTACK1:
		case ACT_VM_MELEE_ATTACK2:
		case ACT_VM_MELEE_ATTACK3:
		case ACT_VM_MELEE_LIGHT:
		case ACT_VM_MELEE_HEAVY:
		case ACT_VM_MELEE_SPECIAL:
		case ACT_VM_MELEE_SLAM:
			TitanSword_ClearMods( weapon )
			break
	}

	
	

	
	
	
}

var function OnWeaponPrimaryAttackAnimEvent_weapon_titan_sword( entity weapon, WeaponPrimaryAttackParams params )
{
	TitanSword_Launcher_TryLauncherAnimEvent(weapon)
	TitanSword_Slam_TrySlamAnimEvent(weapon)

	return 0
}

void function TitanSword_SafelyAddAttackMod( entity weapon, string mod )
{

		if ( !InPrediction() )
			return


	
	TitanSword_ClearMods(weapon)

	
	weapon.AddMod( mod )
}

void function TitanSword_ClearMods( entity weapon )
{

		if ( !InPrediction() )
			return

	TitanSword_Block_ClearMods( weapon )
	TitanSword_Dash_ClearMods( weapon )
	TitanSword_Slam_ClearMods( weapon )
	TitanSword_Heavy_ClearMods( weapon )
	TitanSword_Launcher_ClearMods( weapon )
	TitanSword_Super_ClearMods( weapon )

	TitanSword_Light_StopVFX( weapon )
	weapon.RemoveMod( TITAN_SWORD_LIGHT_SUPER_MOD )
}

entity function TitanSword_GetMainWeapon( entity player )
{
	entity weapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
	if ( !IsValid( weapon ) )
		return null

	if ( !TitanSword_WeaponIsTitanSword( weapon ) )
		return null

	return weapon
}

bool function TitanSword_ActiveWeaponIsTitanSword( entity player )
{
	entity weapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
	if ( !IsValid( weapon ) )
		return false

	return TitanSword_WeaponIsTitanSword( weapon )
}

bool function TitanSword_WeaponIsTitanSword( entity weapon )
{
	return TitanSword_WeaponRefIsTitanSword( weapon.GetWeaponClassName() )
}

bool function TitanSword_WeaponRefIsTitanSword( string ref )
{
	return ref == TITAN_SWORD_WEAPON_REF
}

bool function TitanSword_DamageSourceIsTitanSword( int damageSourceId )
{
	return damageSourceId == eDamageSourceId.mp_weapon_titan_sword || damageSourceId == eDamageSourceId.melee_titan_sword || damageSourceId == eDamageSourceId.mp_weapon_titan_sword_slam
}


bool function TitanSword_ClientPredictCheck( string power )
{
	
	if ( !GetCurrentPlaylistVarBool( "titan_sword_predict_" + power, true ) )
		return true

	
	if ( !InPrediction() || (InPrediction() && IsFirstTimePredicted()) )
		return true

	return false
}






































































































































































































































void function TitanSword_ClearHints()
{
	HidePlayerHint( "#WPN_TITAN_SWORD_SUPER_READY_HINT" )
	HidePlayerHint( "#WPN_TITAN_SWORD_HINT" )
	HidePlayerHint( "#WPN_TITAN_SWORD_DASH_HINT" )
	HidePlayerHint( "#WPN_TITAN_SWORD_SLAM_HINT" )
	HidePlayerHint( "#WPN_TITAN_SWORD_DASH_NOT_READY" )
}

void function TitanSword_DisplayHint( entity player, string message, float time = 6.0, float debounce = 0.0 )
{
	if ( !IsLocalViewPlayer( player ) )
		return

	switch( message )
	{
		case "#WPN_TITAN_SWORD_SLAM_HINT":
		case "#WPN_TITAN_SWORD_DASH_HINT":
			if ( Time() < file.debounceInstructionMsg )
				return
			file.debounceInstructionMsg = Time() + debounce
			break

		case "#WPN_TITAN_SWORD_HINT":
			if ( Time() < file.debounceMainMsg )
				return
			file.debounceMainMsg = Time() + debounce
			break

		default:
			break
	}

	TitanSword_ClearHints()
	AddPlayerHint( time, 0.5, $"", message )
}




void function OnPrimaryWeaponStatusUpdate_TitanSword( entity selectedWeapon, var weaponRui )
{
	if ( !IsValid( selectedWeapon ) )
		return

	
	entity activeWeapon = GetLocalViewPlayer().GetActiveWeapon( eActiveInventorySlot.mainHand  )
	bool switchToMeleeOrGrenade = IsBitFlagSet( selectedWeapon.GetWeaponTypeFlags(), ( WPT_VIEWHANDS | WPT_GRENADE ) )
	if ( IsValid( activeWeapon ) && activeWeapon != selectedWeapon )
	{
		if ( !( TitanSword_WeaponIsTitanSword( activeWeapon ) && switchToMeleeOrGrenade ) )
			activeWeapon.Signal( SIG_TITAN_SWORD_DESTROY_FUEL_RUI )
	}

	if ( TitanSword_WeaponIsTitanSword( selectedWeapon ) )
	{
		entity player = selectedWeapon.GetWeaponOwner()
		thread TitanSword_FuelMeterRui_Thread( selectedWeapon, player )
	}
}


void function TitanSword_UpdateFuelCrosshair( entity player, entity weapon )
{

		if ( !InPrediction() || !IsFirstTimePredicted() )
			return

	float scale = TitanSword_GetFuelScale( player )
	weapon.SetWeaponPrimaryClipCountNoRegenReset( weapon.GetWeaponSettingInt( eWeaponVar.ammo_clip_size ) * scale )
	
	
}

bool function TitanSword_TryUseFuel( entity player, bool playMessage = false )
{
	if ( !TitanSword_HasFuel( player ) )
	{

			if ( Time() > file.debounceDashMsg )
			{
				
				if ( playMessage )
				{
					EmitSoundOnEntity( player, SFX_TITAN_SWORD_FUEL_NOT_READY )
					TitanSword_DisplayHint( player, "#WPN_TITAN_SWORD_DASH_NOT_READY", TITAN_SWORD_DASH_NOT_READY_DEBOUNCE_TIME_SEC + 0.5 )
				}
				file.debounceDashMsg = Time() + TITAN_SWORD_DASH_NOT_READY_DEBOUNCE_TIME_SEC
			}

		return false
	}
	if ( !TitanSword_Super_IsActive( player ) )
	{

			file.debounceDashMsg = Time() + TITAN_SWORD_DASH_NOT_READY_DEBOUNCE_TIME_SEC


		float fuelCooldown = GetWeaponInfoFileKeyField_GlobalFloat( TITAN_SWORD_WEAPON_REF, "fuel_cooldown_sec" )
		StatusEffect_AddTimed( player, eStatusEffect.titan_sword_fuel, 1.0, fuelCooldown, 0.0 )

		entity weapon = TitanSword_GetMainWeapon( player )
		if ( IsValid( weapon ) )
			TitanSword_UpdateFuelCrosshair( player, weapon )







	}
	return true
}

bool function TitanSword_HasFuel( entity player )
{
	return !StatusEffect_HasSeverity( player, eStatusEffect.titan_sword_fuel ) || TitanSword_Super_IsActive( player )
}

void function TitanSword_FillFuel( entity player )
{
	StatusEffect_StopAllOfType( player, eStatusEffect.titan_sword_fuel )
	entity weapon = TitanSword_GetMainWeapon( player )
	if ( IsValid( weapon ) )
		TitanSword_UpdateFuelCrosshair( player, weapon )
}

float function TitanSword_GetFuelScale( entity player )
{
	if ( TitanSword_HasFuel( player ) )
		return 1.0

	float fuel = StatusEffect_GetTimeRemaining( player, eStatusEffect.titan_sword_fuel )
	float diff = fuel / GetWeaponInfoFileKeyField_GlobalFloat( TITAN_SWORD_WEAPON_REF, "fuel_cooldown_sec" )
	return 1.0 - diff
}



void function TitanSword_OnFuelFull( entity player, int statusEffect, bool actuallyChanged )
{
	if ( !actuallyChanged )
		return

	if ( !IsLocalViewPlayer( player ) )
		return

	EmitSoundOnEntity( player, SFX_TITAN_SWORD_FUEL_READY )
}



var function TitanSword_GetFuelRui()
{
	return file.fuelRui
}

void function TitanSword_FuelMeterRui_Thread( entity weapon, entity player )
{
	if ( !IsValid( weapon ) )
		return

	if ( weapon != player.GetActiveWeapon( eActiveInventorySlot.mainHand ) )
		return

	if ( !IsValid( player ) || !IsLocalViewPlayer( player ) )
		return

	if ( file.fuelRui != null )
		return

	weapon.Signal( SIG_TITAN_SWORD_DESTROY_FUEL_RUI )
	weapon.EndSignal( "OnDestroy" )
	weapon.EndSignal( SIG_TITAN_SWORD_DESTROY_FUEL_RUI )
	weapon.EndSignal ( SIG_TITAN_SWORD_DEACTIVATE )

	player.EndSignal( "OnDeath" )
	player.EndSignal( "OnDestroy" )


	file.fuelRui = CreateCockpitRui( RUI_TITAN_SWORD_FUEL_HUD )
	OnThreadEnd(
		function() : ()
		{
			if ( file.fuelRui != null )
			{
				RuiDestroyIfAlive( file.fuelRui )
				file.fuelRui = null
			}
		}
	)

	while( IsValid( player ) && IsValid( weapon ) )
	{
		if ( file.fuelRui == null )
			return

		float dashFrac = TitanSword_GetFuelScale( player )
		bool fuelFull = dashFrac >= 1.0
		bool hasUnlimitedDash = TitanSword_Super_IsActive( player )
		bool isBlocking = TitanSword_Block_IsBlocking( weapon )
		bool canLaunch = fuelFull && player.IsOnGround()
		string hintText = ""

		if ( isBlocking )
		{
			if ( fuelFull || hasUnlimitedDash )
			{
				hintText = "#WPN_TITAN_SWORD_PROMPT_DASH"
			}
			else
			{
				hintText = "#WPN_TITAN_SWORD_DASH_NOT_READY"
			}
		}
		else
		{
			if ( canLaunch )
			{
				hintText = "#WPN_TITAN_SWORD_PROMPT_LAUNCH"
			}
			else
			{
				hintText = "#WPN_TITAN_SWORD_PROMPT_LIGHT"
			}
		}

		RuiSetFloat( file.fuelRui, "dashFrac", dashFrac )
		RuiSetBool( file.fuelRui, "hasUnlimitedDash", hasUnlimitedDash )
		RuiSetString( file.fuelRui, "hintText", hintText )

		WaitFrame()
	}
}



bool function TitanSword_VictimHitOverride( entity weapon, entity attacker, entity victim, vector velocity )
{
	if ( TitanSword_Launcher_VictimHitOverride( weapon, attacker, victim, velocity ) )
		return true

	if ( TitanSword_Slam_VictimHitOverride( weapon, attacker, victim, velocity ) )
		return true

	if ( TitanSword_Light_VictimHitOverride( weapon, attacker, victim, velocity ) )
		return true

	return false
}

bool function TitanSword_Light_VictimHitOverride( entity weapon, entity attacker, entity victim, vector velocity )
{
	
	
	

	if ( !IsFriendlyTeam( attacker.GetTeam(), victim.GetTeam() ) )
	{
		if ( !victim.IsOnGround() && !attacker.IsOnGround() )
		{
			TitanSword_LaunchEntity( attacker, <0, 0, 150> )
			TitanSword_LaunchEntity( victim, <0, 0, 250> )
			return true
		}
	}
	return false
}

void function TitanSword_AirCombo_Thread( entity ent )
{
	printt( "AIR COMBO THREAD: " + ent )













}

void function TitanSword_LaunchEntity( entity victim, vector velocity )
{
	if ( victim.IsPlayer() )
	{
		victim.PlayerLaunch( velocity, false )
	}






}

#if DEV



























#endif


int function TitanSword_CanGoThroughBlockingEntity( entity blockingEntity )
{
	if ( !IsValid( blockingEntity ) )
		return GOING_THROUGH_TRUE

	

	
	if ( blockingEntity.GetScriptName() == BUBBLE_SHIELD_SCRIPTNAME )
	{
		return GOING_THROUGH_TRUE
	}

	
	if ( blockingEntity.GetScriptName() == MOBILE_SHIELD_SCRIPTNAME )
	{
		return GOING_THROUGH_TRUE
	}

	
	if ( blockingEntity.GetScriptName() == AMPED_WALL_SCRIPT_NAME )
	{
		return GOING_THROUGH_TRUE
	}

	
	if ( blockingEntity.GetScriptName() == BASE_WALL_SCRIPT_NAME )
	{
		return GOING_THROUGH_FORCE_HIT
	}

	
	if ( blockingEntity.GetScriptName() == TROPHY_SYSTEM_NAME )
	{
		return GOING_THROUGH_FORCE_HIT
	}

	
	if ( blockingEntity.GetScriptName() == BLACKHOLE_PROP_SCRIPTNAME )
	{
		return GOING_THROUGH_FORCE_HIT
	}

	
	if ( blockingEntity.GetScriptName() == CARE_PACKAGE_SCRIPTNAME )
	{
		return GOING_THROUGH_FORCE_HIT
	}

	
	if ( blockingEntity.GetScriptName() == MOBILE_RESPAWN_BEACON_TARGETNAME )
	{
		return GOING_THROUGH_FORCE_HIT
	}

	
	
	if ( blockingEntity.IsWeaponX() )
	{
		if ( blockingEntity.GetWeaponClassName() == MOUNTED_TURRET_WEAPON_NAME ||
				blockingEntity.GetWeaponClassName() == COVER_WALL_WEAPON_NAME )
		{
			return GOING_THROUGH_TRUE
		}
	}

	if ( blockingEntity.GetModelName() == COLLISION_CYLINDER_MODEL )
	{
		return GOING_THROUGH_TRUE
	}

	if ( blockingEntity.GetScriptName() == MOUNTED_TURRET_PLACEABLE_SCRIPT_NAME )
	{
		return GOING_THROUGH_FORCE_HIT
	}
	

	return GOING_THROUGH_FALSE
}

                               