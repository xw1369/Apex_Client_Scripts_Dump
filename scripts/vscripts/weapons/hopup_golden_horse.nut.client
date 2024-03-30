
global function HopupGoldenHorse_Init
global function HopupGoldenHorse_Switcheroo
global function HopupGoldenHorse_Switcheroo_LockedSets

global function GoldenHorseGreen_OnWeaponActivate
global function GoldenHorseGreen_OnWeaponDeactivate
global function GoldenHorseGreen_OnWeaponReload
global function GoldenHorseGreen_OnWeaponReloadFinished

global function GoldenHorsePurple_OnWeaponPrimaryAttack
global function GoldenHorsePurple_PostFire
global function GoldenHorsePurple_HasMod

global function GoldenHorseBlue_HasMod

global function HopupGoldenHorse_GetEnabledList


global function HopupGoldenHorse_SwapLootTier







global function ServerToClient_TryDisplayLootHint
global function GoldenHorseGreen_CockpitExplodeVFX
global function GoldenHorseGreen_StartWeaponVFX
global function GoldenHorseGreen_StopWeaponVFX
global function GoldenHorseGreen_StartHUD
global function GoldenHorseGreen_StopHUD
global function GoldenHorseGreen_HUDExplode

global function GoldenHorseRed_ShowRui
global function GoldenHorseRed_HideRui


#if DEV






#endif


const string GH_BLUE = "blue"
const string GH_GREEN = "green"
const string GH_YELLOW = "yellow"
const string GH_PURPLE = "purple"
const string GH_RED = "red"



const string PVAR_GOLDEN_HORSE_HOPUP_ENABLED = "golden_horse_enabled_"
const string PVAR_GOLDEN_HORSE_RED_USE = "golden_horse_red_allow_use"

const string PVAR_GOLDEN_HORSE_RED_FOLLOW_MOVE = "golden_horse_red_follow_move"
const string PVAR_GOLDEN_HORSE_RED_FOLLOW_COMBAT = "golden_horse_red_follow_combat"
const string PVAR_GOLDEN_HORSE_RED_FOLLOW_GOAL = "golden_horse_red_follow_goal"
const string PVAR_GOLDEN_HORSE_RED_ENEMY_DIST = "golden_horse_red_enemy_dist"
const string PVAR_GOLDEN_HORSE_RED_COOLDOWN = "golden_horse_red_cooldown_sec"
const string PVAR_GOLDEN_HORSE_RED_TELEPORT_DIST = "golden_horse_red_teleport_dist"
const string PVAR_GOLDEN_HORSE_RED_TELEPORT_COMBAT_DIST = "golden_horse_red_teleport_combat_dist"


const string GOLDEN_HORSE_MOD = "hopup_golden_horse_"

const string GOLDEN_HORSE_MOD_BLUE = GOLDEN_HORSE_MOD + GH_BLUE
const string GOLDEN_HORSE_MOD_GREEN = GOLDEN_HORSE_MOD + GH_GREEN
const string GOLDEN_HORSE_MOD_YELLOW = GOLDEN_HORSE_MOD + GH_YELLOW
const string GOLDEN_HORSE_MOD_PURPLE = GOLDEN_HORSE_MOD + GH_PURPLE
const string GOLDEN_HORSE_MOD_RED = GOLDEN_HORSE_MOD + GH_RED

const string GOLDEN_HORSE_ACTIVE_MOD = "hopup_golden_horse_active_"


const string SIG_GOLDEN_HORSE_RED_EMPTY = "golden_horse_red_empty"
const string SIG_GOLDEN_HORSE_RED_HIDE_RUI = "golden_horse_red_hide_rui"
const string SIG_GOLDEN_HORSE_GREEN_STOP_HURT_VFX = "golden_horse_green_stop_screen_vfx"
const string SIG_GOLDEN_HORSE_GREEN_STOP_HUD = "golden_horse_green_stop_hud"


const bool DEBUG_REQUIRE_HOPUP = false


const float GOLDEN_HORSE_RED_MAX_SUMMON_PER_PLAYER = 2
const float GOLDEN_HORSE_RED_SUMMON_SPAWN_TIME_SEC = 1
const float GOLDEN_HORSE_RED_SUMMON_COOLDOWN_TIME_SEC = 15
const float GOLDEN_HORSE_RED_DIST_TIME_SEC = 5
const float GOLDEN_HORSE_RED_MAX_DIST = 75 * METERS_TO_INCHES
const float GOLDEN_HORSE_RED_MAX_DIST_COMBAT = 200 * METERS_TO_INCHES
const float GOLDEN_HORSE_RED_MAX_DIST_SQR = GOLDEN_HORSE_RED_MAX_DIST * GOLDEN_HORSE_RED_MAX_DIST
const float GOLDEN_HORSE_RED_MAX_DIST_COMBAT_SQR = GOLDEN_HORSE_RED_MAX_DIST_COMBAT * GOLDEN_HORSE_RED_MAX_DIST_COMBAT
const float GOLDEN_HORSE_RED_FOLLOW_MOVE = 256 
const float GOLDEN_HORSE_RED_FOLLOW_COMBAT = 1024 
const float GOLDEN_HORSE_RED_FOLLOW_GOAL = 500 
const float GOLDEN_HORSE_RED_ENEMY_DIST = 150 * METERS_TO_INCHES



const asset MDL_GOLDEN_HORSE_NESSIE = $"mdl/props/nessie/nessie_ragold_w.rmdl"



const asset VFX_GOLDEN_HORSE_GREEN_CHARGE_3P = $"P_Gmat_exp_buildup" 


const asset VFX_GOLDEN_HORSE_GREEN_EXPLODE_COCKPIT = $"P_gmat_exp_FP_shockwave"
const asset VFX_GOLDEN_HORSE_GREEN_EXPLODE_1P = $"P_gmat_exp_FP_dlight"
const asset VFX_GOLDEN_HORSE_GREEN_EXPLODE_3P = $"P_Gmat_exp_shockwave"
const asset VFX_GOLDEN_HORSE_GREEN_WEAPON_3P = $"P_Gmat_Gun_arcs"

const asset VFX_GOLDEN_HORSE_RED_SUMMON = $"P_Rmat_warp_spawn"  
const asset VFX_GOLDEN_HORSE_RED_TELEPORT_START = $"P_Rmat_warp_out" 
const asset VFX_GOLDEN_HORSE_RED_TELEPORT_END = $"P_Rmat_warp_in" 

const asset VFX_GOLDEN_HORSE_RED_USE = $"P_Rmat_emote" 


const string SFX_GOLDEN_HORSE_GREEN_CHARGE_1P = "materia_green_emp_charge_start_1P"
const string SFX_GOLDEN_HORSE_GREEN_CHARGE_3P = "materia_green_emp_charge_start_3P"
const string SFX_GOLDEN_HORSE_GREEN_EXPLODE_1P = "materia_green_emp_charge_explode_1P"
const string SFX_GOLDEN_HORSE_GREEN_EXPLODE_3P = "materia_green_emp_charge_explode_3P"



const string SFX_GOLDEN_HORSE_RED_SUMMON = "materia_red_nessie_spawn_3P"
const string SFX_GOLDEN_HORSE_RED_TELEPORT_START = "afltm_vocal_death"
const string SFX_GOLDEN_HORSE_RED_TELEPORT_END = "afltm_den_spawn_pt1"

const string SFX_GOLDEN_HORSE_RED_USE = "materia_red_nessie_cuddle_3P"

const string SFX_GOLDEN_HORSE_PURPLE_SUCCESS = "lootmarvin_dispenseloot"


const asset RUI_GOLDEN_HORSE_RED_HUD = $"ui/weapon_hud_charged_nessie.rpak"


const array<string> VALID_WEAPONS = [
	"mp_weapon_wingman", 
	"mp_weapon_wingman_crate", 
	"mp_weapon_semipistol",
	"mp_weapon_autopistol",
	"mp_weapon_shotgun_pistol",
	"mp_weapon_shotgun",
	"mp_weapon_energy_shotgun",
	"mp_weapon_mastiff",
	"mp_weapon_alternator_smg",
	"mp_weapon_r97",
	"mp_weapon_pdw",
	"mp_weapon_pdw_crate",
	"mp_weapon_vinson",
	"mp_weapon_rspn101",
	"mp_weapon_energy_ar",
	"mp_weapon_hemlok",
	"mp_weapon_esaw",
	"mp_weapon_lstar", 
	"mp_weapon_lstar_crate", 
	"mp_weapon_lmg",
	"mp_weapon_g2",
	"mp_weapon_dmr",
	"mp_weapon_defender",
	"mp_weapon_doubletake",
	"mp_weapon_sentinel",
	"mp_weapon_volt_smg",
	"mp_weapon_bow",
	"mp_weapon_3030",
	"mp_weapon_sniper",
	"mp_weapon_dragon_lmg",
	"mp_weapon_car",
	"mp_weapon_nemesis",
]











struct CritData
{
	float critChance
	float whiffs
}

struct {




	table<entity, CritData>   crits

		var redRui
		var greenRui

}file

bool function IsEnabled( string color )
{
	return GetCurrentPlaylistVarBool( PVAR_GOLDEN_HORSE_HOPUP_ENABLED + color, true )
}

bool function HasMod( entity weapon, string mod, string color )
{
	if ( weapon == null )
		return false

	return weapon.HasMod( mod + color )
}

bool function ProjectileHasMod( entity projectile, string mod, string color )
{
	return projectile.HasWeaponMod( mod + color )
}

bool function IsValidProjectileWithMod( entity projectile, string mod, string color )
{
	if ( !IsValid( projectile ) )
		return false

	if ( !projectile.IsProjectile() )
		return false

	if ( !ProjectileHasMod( projectile, mod, color ) )
		return false

	return true
}

void function AddMod( entity weapon, string mod, string color )
{
	weapon.AddMod( mod + color )
}

void function RemoveMod( entity weapon, string mod, string color )
{
	weapon.RemoveMod( mod + color )
}

array<string> function HopupGoldenHorse_GetEnabledList()
{
	array<string> hopups
	if ( IsEnabled( GH_BLUE ) ) 
		hopups.append( GOLDEN_HORSE_MOD + GH_BLUE )
	if ( IsEnabled( GH_GREEN ) ) 
		hopups.append( GOLDEN_HORSE_MOD + GH_GREEN )
	if ( IsEnabled( GH_YELLOW ) ) 
		hopups.append( GOLDEN_HORSE_MOD + GH_YELLOW )
	if ( IsEnabled( GH_PURPLE ) ) 
		hopups.append( GOLDEN_HORSE_MOD + GH_PURPLE )
	if ( IsEnabled( GH_RED ) ) 
		hopups.append( GOLDEN_HORSE_MOD + GH_RED )

	return hopups
}

void function HopupGoldenHorse_Init()
{
	
	
	if ( !IsGoldenHorse() )
	{
		HopupGoldenHorse_DisableAll()
		return
	}





	if ( IsEnabled( GH_BLUE ) ) 
		GoldenHorseBlue_Init()
	if ( IsEnabled( GH_GREEN ) ) 
		GoldenHorseGreen_Init()
	if ( IsEnabled( GH_YELLOW ) ) 
		GoldenHorseYellow_Init()
	if ( IsEnabled( GH_PURPLE ) ) 
		GoldenHorsePurple_Init()
	if ( IsEnabled( GH_RED ) ) 
		GoldenHorseRed_Init()

	var dt      = GetDataTable( LOOT_DATATABLE )
	int numRows = GetDataTableRowCount( dt )
	Remote_RegisterClientFunction( "ServerToClient_TryDisplayLootHint", "int", 0, numRows )






		AddCallback_EditLootDesc( HopupGoldenHorse_EditWeaponDescription )

}

void function HopupGoldenHorse_DisableAll()
{
	SURVIVAL_Loot_AddDisabledRef( GOLDEN_HORSE_MOD + GH_BLUE )
	SURVIVAL_Loot_AddDisabledRef( GOLDEN_HORSE_MOD + GH_YELLOW )
	SURVIVAL_Loot_AddDisabledRef( GOLDEN_HORSE_MOD + GH_PURPLE )
	SURVIVAL_Loot_AddDisabledRef( GOLDEN_HORSE_MOD + GH_GREEN )
	SURVIVAL_Loot_AddDisabledRef( GOLDEN_HORSE_MOD + GH_RED )
}


void function HopupGoldenHorse_Switcheroo( LootData wData )
{
	if ( !IsGoldenHorse() )
		return

	

	
	switch( wData.ref )
	{
		case "mp_weapon_bow":
			
			wData.supportedAttachments.append( "hopup" )
			wData.supportedAttachments.fastremovebyvalue( "hopupMulti_a" )
			wData.baseMods.fastremovebyvalue( "hopup_shatter_rounds" )
			wData.baseMods.append( GOLDEN_HORSE_MOD + GH_PURPLE )
			break

		case "mp_weapon_wingman_crate":
			
			
			wData.baseMods.fastremovebyvalue( "hopup_headshot_dmg_elite" )
			wData.baseMods.fastremovebyvalue( "hopup_smart_reload" )
			wData.baseMods.append( "hopup_headshot_dmg_elite_ghorse" )
			wData.baseMods.append( GOLDEN_HORSE_MOD + GH_BLUE )
			break

		case "mp_weapon_lstar_crate":
			wData.baseMods.append( GOLDEN_HORSE_MOD + GH_GREEN )
			break

		case "mp_weapon_pdw_crate":
			
			wData.supportedAttachments.fastremovebyvalue( "hopup" )
			wData.supportedAttachments.append( "hopupMulti_a" )
			wData.supportedAttachments.append( "hopupMulti_b" )
			wData.baseMods.append( GOLDEN_HORSE_MOD + GH_GREEN )
			break

		case "mp_weapon_sniper":
			wData.supportedAttachments.append( "hopup" )
			wData.baseMods.append( GOLDEN_HORSE_MOD + GH_RED )
			break

		default:
			if ( VALID_WEAPONS.contains( wData.ref ) )
			{
				if ( !wData.supportedAttachments.contains( "hopup" ) )
					wData.supportedAttachments.append( "hopup" )
			}
			else
			{
				if ( wData.supportedAttachments.contains( "hopup" ) )
					wData.supportedAttachments.fastremovebyvalue( "hopup" )
			}
			break
	}
}

void function HopupGoldenHorse_Switcheroo_LockedSets( LootData wData )
{
	if ( !IsGoldenHorse() )
		return

	if ( wData.baseWeapon == wData.ref )
		return

	string weaponRef = wData.ref
	bool isGold      = weaponRef.find( "_gold" ) != -1 && weaponRef.find( "_paint" ) == -1

	if ( isGold )
	{
		
		for ( int i = 0; i < wData.baseMods.len(); ++i )
		{
			if ( wData.baseMods[i].find( "hopup" ) != -1 )
			{
				wData.baseMods.remove( i )
				--i
			}
		}

		array<string> hopups = GetAttachmentsForPoint( "hopup", wData.baseWeapon )

		
		foreach ( string hopup in hopups )
		{
			if ( hopup.find( GOLDEN_HORSE_MOD ) != -1 )
			{
				wData.baseMods.append( hopup )
				break
			}
		}

		if ( VALID_WEAPONS.contains( wData.baseWeapon ) )
		{
			if ( !wData.supportedAttachments.contains( "hopup" ) )
				wData.supportedAttachments.append( "hopup" )
		}
		else
		{
			if ( wData.supportedAttachments.contains( "hopup" ) )
				wData.supportedAttachments.fastremovebyvalue( "hopup" )
		}
	}
}


string function HopupGoldenHorse_EditWeaponDescription( string lootRef, entity player, string originalDesc )
{
	if ( !IsGoldenHorse() )
		return originalDesc

	array<string> hopups = GetAttachmentsForPoint( "hopup", lootRef )

	foreach ( string hopup in hopups )
	{
		switch( hopup )
		{
			case GOLDEN_HORSE_MOD + GH_GREEN:
				return originalDesc + "\n" + Localize( "#WPN_HOPUP_GOLDEN_HORSE_GREEN_APPEND" )

			case GOLDEN_HORSE_MOD + GH_BLUE:
				return originalDesc + "\n" + Localize( "#WPN_HOPUP_GOLDEN_HORSE_BLUE_APPEND" )

			case GOLDEN_HORSE_MOD + GH_YELLOW:
				return originalDesc + "\n" + Localize( "#WPN_HOPUP_GOLDEN_HORSE_YELLOW_APPEND" )

			case GOLDEN_HORSE_MOD + GH_PURPLE:
				return originalDesc + "\n" + Localize( "#WPN_HOPUP_GOLDEN_HORSE_PURPLE_APPEND" )

			case GOLDEN_HORSE_MOD + GH_RED:
				return originalDesc + "\n" + Localize( "#WPN_HOPUP_GOLDEN_HORSE_RED_APPEND" )
		}
	}

	return originalDesc
}










































bool function HopupGoldenHorse_SwapLootTier( var rui, string ref, int tier, string ruiVar = "lootTier" )
{
	if ( ref == GOLDEN_HORSE_MOD + GH_GREEN )
	{
		RuiSetInt( rui, ruiVar, GOLDEN_HORSE_SPECIAL_EVENT_LOOT_TIER )
		return true
	}
	RuiSetInt( rui, ruiVar, tier )
	return false
}



void function ServerToClient_TryDisplayLootHint( int lootIndex )
{
	Assert( SURVIVAL_Loot_IsLootIndexValid( lootIndex ) )

	if ( !SURVIVAL_Loot_IsLootIndexValid( lootIndex ) )
		return

	LootData lootData = SURVIVAL_Loot_GetLootDataByIndex( lootIndex )
	string localize   = "#WPN_" + lootData.ref.toupper() + "_SEARCH"

	HidePlayerHint( "#WPN_HOPUP_GOLDEN_HORSE_BLUE_SEARCH" )
	HidePlayerHint( "#WPN_HOPUP_GOLDEN_HORSE_GREEN_SEARCH" )
	HidePlayerHint( "#WPN_HOPUP_GOLDEN_HORSE_YELLOW_SEARCH" )
	HidePlayerHint( "#WPN_HOPUP_GOLDEN_HORSE_PURPLE_SEARCH" )
	HidePlayerHint( "#WPN_HOPUP_GOLDEN_HORSE_RED_SEARCH" )

	AddPlayerHint( 6, 0.5, lootData.hudIcon, localize )
}


asset function HopupGoldenHorse_GetIconFor( string color )
{
	switch( color )
	{
		case GH_GREEN:
			return $"rui/pilot_loadout/mods/hopup_golden_horse_green"

		case GH_BLUE:
			return  $"rui/pilot_loadout/mods/hopup_golden_horse_blue"

		case GH_PURPLE:
			return  $"rui/pilot_loadout/mods/hopup_golden_horse_purple"

		case GH_YELLOW:
			return $"rui/hud/ultimate_icons/hopup_golden_horse_yellow"

		case GH_RED:
			return $"rui/hud/ultimate_icons/hopup_golden_horse_red"
	}

	return $"rui/pilot_loadout/mods/empty_hopup"
}


void function GoldenHorseBlue_Init()
{
	
}

bool function GoldenHorseBlue_HasMod( entity weapon )
{
	return HasMod( weapon, GOLDEN_HORSE_MOD, GH_BLUE )
}


void function GoldenHorseGreen_Init()
{
	PrecacheParticleSystem( VFX_GOLDEN_HORSE_GREEN_CHARGE_3P )
	PrecacheParticleSystem( VFX_GOLDEN_HORSE_GREEN_EXPLODE_COCKPIT )
	PrecacheParticleSystem( VFX_GOLDEN_HORSE_GREEN_EXPLODE_1P )
	PrecacheParticleSystem( VFX_GOLDEN_HORSE_GREEN_EXPLODE_3P )
	PrecacheParticleSystem( VFX_GOLDEN_HORSE_GREEN_WEAPON_3P )

	RegisterSignal( SIG_GOLDEN_HORSE_GREEN_STOP_HURT_VFX )
	RegisterSignal( SIG_GOLDEN_HORSE_GREEN_STOP_HUD )
	Remote_RegisterClientFunction( "GoldenHorseGreen_StartWeaponVFX", "entity" )
	Remote_RegisterClientFunction( "GoldenHorseGreen_StopWeaponVFX", "entity" )
	Remote_RegisterClientFunction( "GoldenHorseGreen_StartHUD", "entity" )
	Remote_RegisterClientFunction( "GoldenHorseGreen_StopHUD", "entity" )
	Remote_RegisterClientFunction( "GoldenHorseGreen_HUDExplode" )
	Remote_RegisterClientFunction( "GoldenHorseGreen_CockpitExplodeVFX" )





}

void function GoldenHorseGreen_StartWeaponVFX( entity weapon )
{
	weapon.PlayWeaponEffect( $"", VFX_GOLDEN_HORSE_GREEN_WEAPON_3P, "muzzle_flash" )
}

void function GoldenHorseGreen_StopWeaponVFX( entity weapon )
{
	weapon.StopWeaponEffect( $"", VFX_GOLDEN_HORSE_GREEN_WEAPON_3P )
}

void function GoldenHorseGreen_OnWeaponActivate( entity weapon )
{
	if ( !HasMod( weapon, GOLDEN_HORSE_MOD, GH_GREEN ) )
		return

	GoldenHorseGreen_StartWeaponVFX( weapon )

		if ( weapon.IsWeaponActivated() )
			GoldenHorseGreen_StartHUD( weapon )

}

void function GoldenHorseGreen_OnWeaponDeactivate( entity weapon )
{
	if ( !HasMod( weapon, GOLDEN_HORSE_MOD, GH_GREEN ) )
		return

	GoldenHorseGreen_StopWeaponVFX( weapon )

		GoldenHorseGreen_StopHUD( weapon )

}


void function GoldenHorseGreen_StartHUD( entity weapon )
{
	thread GoldenHorseGreen_HUDThread( weapon )
}

void function GoldenHorseGreen_HUDThread( entity weapon )
{
	if ( !IsValid( weapon ) || !HasMod( weapon, GOLDEN_HORSE_MOD, GH_GREEN ) )
		return

	entity player = weapon.GetWeaponOwner()

	if ( !IsValid( player ) || !IsLocalViewPlayer( player ) )
		return

	player.EndSignal( "OnDeath" )
	player.EndSignal( "OnDestroy" )

	weapon.Signal( SIG_GOLDEN_HORSE_GREEN_STOP_HUD )
	weapon.EndSignal( "OnDestroy" )
	weapon.EndSignal( SIG_GOLDEN_HORSE_GREEN_STOP_HUD )

	var crosshairRui = CreateCockpitRui( $"ui/ammo_status_hint.rpak", HUD_Z_BASE )
	var chargeBarRui = CreateCockpitRui( $"ui/gh_green_crosshair.rpak" )
	if ( chargeBarRui == null || crosshairRui == null )
		return

	file.greenRui = chargeBarRui

	OnThreadEnd(
		function() : ( crosshairRui, chargeBarRui )
		{
			RuiDestroy( crosshairRui )
			RuiDestroy( chargeBarRui )
			file.greenRui = null
		}
	)

	int clipCount
	int maxClipCount
	float clipCountFrac = 1.0
	float delay         = GetWeaponInfoFileKeyField_GlobalFloat( weapon.GetWeaponBaseClassName(), "golden_horse_green_delay_sec" )

	RuiSetFloat( chargeBarRui, "flashDelay", delay )

	while ( true )
	{
		if ( chargeBarRui == null || crosshairRui == null )
			break

		bool showHud = IsValid( weapon ) && HasMod( weapon, GOLDEN_HORSE_MOD, GH_GREEN )
		RuiSetBool( chargeBarRui, "isVisible", showHud )
		RuiSetBool( crosshairRui, "isVisible", showHud )

		if ( !showHud )
		{
			WaitFrame()
			continue
		}

		clipCount    = weapon.GetWeaponPrimaryClipCount()
		maxClipCount = weapon.GetWeaponPrimaryClipCountMax()
		if ( weapon.GetWeaponBaseClassName() == "mp_weapon_lstar" ) 
		{
			clipCountFrac = 1.0 - weapon.GetWeaponChargeFraction()
		}
		else
		{
			clipCountFrac = float( clipCount) / float( maxClipCount )
		}

		if ( clipCountFrac == 0.0 )
			ClWeaponStatus_OverrideReloadHintText( "#WPN_HOPUP_GOLDEN_HORSE_GREEN_RELOAD_HINT" )
		else
			ClWeaponStatus_OverrideReloadHintText( "" )

		RuiSetFloat( chargeBarRui, "clipCountFrac", clipCountFrac )

		WaitFrame()
	}
}

void function GoldenHorseGreen_HUDExplode()
{
	if ( file.greenRui == null )
		return

	RuiSetGameTime( file.greenRui, "flashStartTime", Time() )
	RuiSetBool( file.greenRui, "flashState", true )
}

void function GoldenHorseGreen_StopHUD( entity weapon )
{
	if ( !IsValid( weapon ) )
		return

	weapon.Signal( SIG_GOLDEN_HORSE_GREEN_STOP_HUD )
}






























void function GoldenHorseGreen_OnPlayerRemoveWeaponMod( entity player, entity weapon, string mod )
{
	printt( "WEAPON MOD REMOVED: " + weapon + " :: " + mod )

	if ( mod == GOLDEN_HORSE_MOD + GH_GREEN )
	{
		if ( !IsValid( weapon ) )
			return

		if ( !weapon.IsWeaponX() )
			return

		GoldenHorseGreen_StopWeaponVFX( weapon )

			GoldenHorseGreen_StopHUD( weapon )

	}
}

void function GoldenHorseGreen_OnWeaponReload( entity weapon, int milestoneIndex )
{




















}

void function GoldenHorseGreen_OnWeaponReloadFinished( entity weapon )
{
	
	return

	if ( !HasMod( weapon, GOLDEN_HORSE_MOD, GH_GREEN ) )
		return




}




























































































































































































void function GoldenHorseGreen_CockpitExplodeVFX()
{
	entity player = GetLocalViewPlayer()

	entity cockpit = player.GetCockpit()
	if ( !IsValid( cockpit ) )
		return

	

	int fxID   = GetParticleSystemIndex( VFX_GOLDEN_HORSE_GREEN_EXPLODE_COCKPIT )
	int handle = StartParticleEffectOnEntity( cockpit, fxID, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetIsWithCockpit( handle, true )
}



void function GoldenHorseYellow_Init()
{



}
























































void function GoldenHorsePurple_Init()
{



}


var function GoldenHorsePurple_OnWeaponPrimaryAttack( entity weapon, WeaponPrimaryAttackParams params )
{

		if ( !InPrediction() )
			return


	if ( !HasMod( weapon, GOLDEN_HORSE_MOD, GH_PURPLE ) )
		return

	float baseChance = GetWeaponInfoFileKeyField_GlobalFloat( weapon.GetWeaponBaseClassName(), "golden_horse_purple_chance" )
	float growth     = GetWeaponInfoFileKeyField_GlobalFloat( weapon.GetWeaponBaseClassName(), "golden_horse_purple_growth" )
	float whiffMax   = GetWeaponInfoFileKeyField_GlobalFloat( weapon.GetWeaponBaseClassName(), "golden_horse_purple_whiff" )
	float reset      = GetWeaponInfoFileKeyField_GlobalFloat( weapon.GetWeaponBaseClassName(), "golden_horse_purple_reset_sec" )

	float rand = RandomFloat( 1.0 )

	CritData data
	if ( !(weapon in file.crits) )
	{
		data.critChance = baseChance
		file.crits[weapon] <- data
		
		AddEntityDestroyedCallback( weapon, GoldenHorsePurple_OnEntityDestroyed )
	}

	data = file.crits[weapon]

	
	const float GOLDEN_HORSE_PURPLE_CRIT_RESET_SEC = 5
	const float GOLDEN_HORSE_PURPLE_CRIT_GROWTH = 0.1

	if ( Time() - weapon.GetLastWeaponFireTime() > reset )
		data.critChance = baseChance

	

	if ( rand < data.critChance )
	{
		
		if ( !HasMod( weapon, GOLDEN_HORSE_ACTIVE_MOD, GH_PURPLE ) )
		{
			AddMod( weapon, GOLDEN_HORSE_ACTIVE_MOD, GH_PURPLE )
			data.critChance = baseChance 
			data.whiffs     = 0
			
			
		}
	}
	else
	{
		data.critChance += growth 
		data.whiffs += 1 

		
		if ( data.whiffs >= whiffMax )
			data.critChance = 1.0
	}
}


void function GoldenHorsePurple_OnEntityDestroyed( entity weapon )
{
	delete file.crits[weapon]
	RemoveEntityDestroyedCallback( weapon, GoldenHorsePurple_OnEntityDestroyed )
	printt( "Weapon Crit recently removed - size: " + file.crits.len() )
}


void function GoldenHorsePurple_PostFire( entity weapon )
{

		if ( !InPrediction() )
			return


	if ( !HasMod( weapon, GOLDEN_HORSE_MOD, GH_PURPLE ) )
		return

	if ( HasMod( weapon, GOLDEN_HORSE_ACTIVE_MOD, GH_PURPLE ) )
		RemoveMod( weapon, GOLDEN_HORSE_ACTIVE_MOD, GH_PURPLE )
}

bool function GoldenHorsePurple_HasMod( entity weapon )
{
	return HasMod( weapon, GOLDEN_HORSE_MOD, GH_PURPLE )
}






























void function GoldenHorseRed_Init()
{
	PrecacheParticleSystem( VFX_GOLDEN_HORSE_RED_SUMMON )
	PrecacheParticleSystem( VFX_GOLDEN_HORSE_RED_TELEPORT_START )
	PrecacheParticleSystem( VFX_GOLDEN_HORSE_RED_TELEPORT_END )
	PrecacheParticleSystem( VFX_GOLDEN_HORSE_RED_USE )

	PrecacheModel( MDL_GOLDEN_HORSE_NESSIE )

	Remote_RegisterClientFunction( "GoldenHorseRed_ShowRui", "entity" )
	Remote_RegisterClientFunction( "GoldenHorseRed_HideRui", "entity" )















		RegisterSignal( SIG_GOLDEN_HORSE_RED_HIDE_RUI )
		StatusEffect_RegisterEnabledCallback( eStatusEffect.golden_horse_red, GoldenHorseRed_OnStatusEffectEnabled )
		StatusEffect_RegisterDisabledCallback( eStatusEffect.golden_horse_red, GoldenHorseRed_OnStatusEffectDisabled )
		AddCallback_OnPlayerWeaponSwitched( GoldenHorseRed_OnPlayerWeaponSwitched )

}

float function GoldenHorseRed_GetCooldown()
{
	return GetCurrentPlaylistVarFloat( PVAR_GOLDEN_HORSE_RED_COOLDOWN, GOLDEN_HORSE_RED_SUMMON_COOLDOWN_TIME_SEC )
}


bool function GoldenHorseRed_HasMod( entity weapon )
{
	return HasMod( weapon, GOLDEN_HORSE_MOD, GH_RED )
}








































































































bool function HasSummonCooldown( entity player )
{
	return StatusEffect_HasSeverity( player, eStatusEffect.golden_horse_red )
}















































































































































































































































































































































































































































































































void function GoldenHorseRed_OnPlayerWeaponSwitched( entity player, entity newWeapon, entity oldWeapon )
{
	if ( DoesModExist( newWeapon, GOLDEN_HORSE_MOD + GH_RED ) )
		GoldenHorseRed_ShowRui( player )
	else
		GoldenHorseRed_HideRui( player )
}

void function GoldenHorseRed_OnStatusEffectEnabled( entity player, int statusEffect, bool actuallyChanged )
{
	if ( player != GetLocalViewPlayer() )
		return

	if ( !actuallyChanged )
		return

	GoldenHorseRed_ShowRui( player )
}

void function GoldenHorseRed_OnStatusEffectDisabled( entity player, int statusEffect, bool actuallyChanged )
{
	if ( player != GetLocalViewPlayer() )
		return

	if ( !actuallyChanged )
		return

	GoldenHorseRed_HideRui( player )
}

void function GoldenHorseRed_ShowRui( entity player )
{
	thread GoldenHorseRed_SummonRui_Thread( player )
}

void function GoldenHorseRed_HideRui( entity player )
{
	player.Signal( SIG_GOLDEN_HORSE_RED_HIDE_RUI )
}

void function GoldenHorseRed_SummonRui_Thread( entity player )
{
	AssertIsNewThread()

	if ( !IsValid( player ) || !IsLocalViewPlayer( player ) )
		return

	entity weapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )

	if ( !IsValid( weapon ) )
		return

	if ( !DoesModExist( weapon, GOLDEN_HORSE_MOD + GH_RED ) ) 
		return

	if ( !HasSummonCooldown( player ) )
		return

	player.Signal( WEAPON_CHARGED_RUI_ABORT_SIGNAL )
	player.Signal( SIG_GOLDEN_HORSE_RED_HIDE_RUI )
	player.EndSignal( "OnDeath" )
	player.EndSignal( "OnDestroy" )
	player.EndSignal( SIG_GOLDEN_HORSE_RED_HIDE_RUI )
	player.EndSignal( WEAPON_CHARGED_RUI_ABORT_SIGNAL )

	weapon.Signal( SIG_GOLDEN_HORSE_RED_HIDE_RUI )
	weapon.EndSignal( SIG_GOLDEN_HORSE_RED_HIDE_RUI )
	weapon.EndSignal( "OnDestroy" )

	var rui = ClWeaponStatus_GetWeaponHudRui( player )

	if ( rui == null )
		return

	RuiDestroyNestedIfAlive( rui, "chargedHandle" )

	var nestedRui = RuiCreateNested( rui, "chargedHandle", RUI_GOLDEN_HORSE_RED_HUD )
	if ( nestedRui == null )
		return

	file.redRui = nestedRui
	OnThreadEnd(
		function() : ( rui, nestedRui )
		{
			RuiSetBool( rui, "showChargeBar", false )
			RuiDestroyNested( rui, "chargedHandle" )
			file.redRui = null
		}
	)

	RuiSetBool( rui, "showChargeBar", true )
	RuiSetBool( nestedRui, "showChargeBar", true )

	while( HasSummonCooldown( player ) && file.redRui != null )
	{
		weapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
		if ( !IsValid( weapon ) || !GoldenHorseRed_HasMod( weapon ) )
			break

		float timeRemaining = StatusEffect_GetTimeRemaining( player, eStatusEffect.golden_horse_red )
		float chargeFrac    = timeRemaining / GoldenHorseRed_GetCooldown()

		RuiSetFloat( nestedRui, "chargeBarTimeRemaining", timeRemaining )
		RuiSetFloat( nestedRui, "chargeBarFrac", chargeFrac )

		WaitFrame()
	}
}































#if DEV

































































#endif

                         