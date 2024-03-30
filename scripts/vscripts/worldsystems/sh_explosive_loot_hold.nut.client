
global function ShExplosiveHold_Init
global function ExplosiveHold_IsOpen
global function ExplosiveHold_PlayerHasGrenadeInInventory


global function ExplosiveHold_IsPlayerPlantingGrenade
global function GetExplosiveHoldProxyForLoot






global const asset EXPLOSIVE_HOLD_PROXY = $"mdl/props/explosivehold_container_01/explosivehold_container_01_proxy.rmdl"
global const string EXPLOSIVE_HOLD_PANEL_SCRIPTNAME = "explosive_hold_panel"

const string EXPLOSIVE_HOLD_SCRIPTNAME = "explosive_hold"
const string EXPLOSIVE_HOLD_MOVER_SCRIPTNAME = "explosive_hold_door_mover"
const string EXPLOSIVE_HOLD_GUN_RACK_SCRIPTNAME = "explosive_hold_gun_rack"
const string EXPLOSIVE_HOLD_PANEL_HOUSING = "explosive_hold_panel_housing"
const string EXPLOSIVE_HOLD_DOOR_RIGHT = "explosive_hold_door_right"
const string EXPLOSIVE_HOLD_DOOR_LEFT = "explosive_hold_door_left"
const string EXPLOSIVE_HOLD_VENT_SMOKE_SCRIPTNAME = "explosive_hold_vent_fx_helper"
const string EXPLOSIVE_HOLD_ATTACHMENTS_PARENT_SCRIPTNAME = "explosive_hold_attachments_parent"

const string EXPLOSIVE_HOLD_WEAPON_LOOT_GROUP = "Weapon_Medium"
global const string EXPLOSIVE_HOLD_ATTACHMENTS_LOOT_GROUP = "Explosive_Hold_Attachments"

const string DOOR_DENY_SOUND = "menu_deny"
const string GRENADE_DETONATE_SOUND = "Loot_ExplosiveHold_Explosion_3p"
const string ARC_PLACEMENT_SOUND = "weapon_arcstar_explosivewarningbeep"
const string PANEL_ALARM_SOUND = "Loot_ExplosiveHold_PanelAlarm_3p"
const string OPEN_DOOR_DAMAGED_SOUND = "Loot_ExplosiveHold_Door_Damaged_Open_3p"
const string OPEN_DOOR_SOUND = "Loot_ExplosiveHold_Door_Back_Open_3p"
const string LOBA_BLACK_MARKET_ALARM_SOUND = "Loba_Ultimate_Staff_VaultAlarm"

const float GRENADE_FUSE = 2.0
const float PANEL_UPWARD_OFFSET = 69.0
const float PANEL_USABLE_DISTANCE_OVERRIDE = 20.0
const float PANEL_USABLE_HEIGHT = 80.0
const float DOOR_TOTAL_TRAVEL_DIST = 50.0
const float EXPLOSIVE_HOLD_MAX_LOOT_DISTANCE = 130

const asset EXPLOSIVE_HOLD_PANEL_ANIM_IDLE = $"animseq/props/explosivehold_panel_animated/explosivehold_panel_idle.rseq"
const asset EXPLOSIVE_HOLD_EXPLOSION_FX = $"P_exp_hold_exp_emp_med"
const asset EXPLOSIVE_HOLD_VENT_SMOKE_FX = $"P_exp_hold_vent_smk_linger_sm"

const int EXPLOSIVE_HOLD_WEAPONS_NEEDED = 6 
const int EXPLOSIVE_HOLD_CRATE_WEAPON_LOOT_TIER = 5


const int EXPLOSIVEHOLD_NOPLACEVOL_RADIUS = 220
const int EXPLOSIVEHOLD_NOPLACEVOL_HEIGHT = 150
const int EXPLOSIVEHOLD_NOPLACEVOL_COUNT = 1


const int EXPLOSIVEHOLD_BREACHTRIGGER_RADIUS = 75
const int EXPLOSIVEHOLD_BREACHTRIGGER_HEIGHT = 75
const int EXPLOSIVEHOLD_BREACHTRIGGER_COUNT = 3
const string EXPLOSIVEHOLD_BREACH_ALARM = "Loba_Ultimate_Staff_VaultAlarm"















const asset EXPLOSIVE_HOLD_ARC_GRENADE_MODEL = $"mdl/weapons_r5/loot/w_loot_wep_iso_shuriken.rmdl"

struct ExplosiveHoldGrenadeData
{
	asset  panelOpenAnim
	string thirdPersonAnim
	string firstPersonAnim
	asset  modelName
	string weaponName
	string targetName

	asset panelOpenAnim_Fuse
	string firstPersonAnim_Fuse
}
const ExplosiveHoldGrenadeData fragGrenadeData = {
	panelOpenAnim = $"animseq/props/explosivehold_panel_animated/explosivehold_panel_open_frag.rseq",
	thirdPersonAnim = "pilot_explosive_hold_start_frag",
	firstPersonAnim = "ptpov_explosive_hold_start_frag",
	modelName = $"mdl/weapons/grenades/m20_f_grenade.rmdl",
	weaponName = "mp_weapon_frag_grenade",
	targetName = "explosive_hold_frag_grenade",

	panelOpenAnim_Fuse = $"animseq/props/explosivehold_panel_animated/explosivehold_panel_open_frag_fuse.rseq",
	firstPersonAnim_Fuse = "ptpov_explosive_hold_start_frag_fuse"
}

const ExplosiveHoldGrenadeData thermiteGrenadeData = {
	panelOpenAnim = $"animseq/props/explosivehold_panel_animated/explosivehold_panel_open_thermite.rseq",
	thirdPersonAnim = "pilot_explosive_hold_start_thermite",
	firstPersonAnim = "ptpov_explosive_hold_start_thermite",
	modelName = $"mdl/weapons/grenades/w_thermite_grenade.rmdl",
	weaponName = "mp_weapon_thermite_grenade",
	targetName = "explosive_hold_thermite_grenade",

	panelOpenAnim_Fuse = $"animseq/props/explosivehold_panel_animated/explosivehold_panel_open_thermite_fuse.rseq",
	firstPersonAnim_Fuse = "ptpov_explosive_hold_start_thermite_fuse"
}

const ExplosiveHoldGrenadeData arcGrenadeData = {
	panelOpenAnim = $"animseq/props/explosivehold_panel_animated/explosivehold_panel_open_shuriken.rseq",
	thirdPersonAnim = "pilot_explosive_hold_start_shuriken",
	firstPersonAnim = "ptpov_explosive_hold_start_shuriken",
	modelName = EXPLOSIVE_HOLD_ARC_GRENADE_MODEL,
	weaponName = GRENADE_EMP_WEAPON_NAME,
	targetName = "explosive_hold_arc_grenade",

	panelOpenAnim_Fuse = $"animseq/props/explosivehold_panel_animated/explosivehold_panel_open_shuriken_fuse.rseq",
	firstPersonAnim_Fuse = "ptpov_explosive_hold_start_shuriken_fuse"
}

struct ExplosiveHoldData
{
	array<entity> panels
	array<entity> panelHousings
	array<entity> lootEnts
	entity rightDoor
	entity leftDoor
	array<entity> ventFXHelpers
	entity holdProxy

	
	array< vector > entranceLocs 
	array< entity > noPlaceVolumes 
	array< entity > breachTriggers 

	entity currentUser 
}

struct DoorInfo
{
	vector moveDir
	entity door
	entity mover
}

struct
{
	array< entity > explosiveHoldEnts 

	array < ExplosiveHoldGrenadeData > grenadeDatas = [ fragGrenadeData, thermiteGrenadeData, arcGrenadeData ]







} file

void function ShExplosiveHold_Init()
{
	AddCallback_EntitiesDidLoad( EntitiesDidLoad )






		AddCreateCallback( "prop_dynamic", OnPanelCreated )


	RegisterSignal( "LootHoldUseDone" )
	RegisterSignal( "LootHoldUseFail" )
	RegisterSignal( "LootHoldUserBootedByBreach" )
	RegisterSignal( "LootHoldConnectionChanged" )
	RegisterSignal( "MaybeActivateExplosiveHoldDefense_Thread" )
}

array< entity > function _ExplosiveHoldEnts_Get()
{
	return( file.explosiveHoldEnts )
}

void function EntitiesDidLoad()
{
	file.explosiveHoldEnts = GetEntArrayByScriptName( EXPLOSIVE_HOLD_SCRIPTNAME )
	if( file.explosiveHoldEnts.len() == 0 )
		return

	PrecacheScriptString( EXPLOSIVE_HOLD_MOVER_SCRIPTNAME )
	PrecacheParticleSystem( EXPLOSIVE_HOLD_EXPLOSION_FX )
	PrecacheParticleSystem( EXPLOSIVE_HOLD_VENT_SMOKE_FX )






















































































































































}











































































































































































































































void function OnPanelCreated( entity panel )
{
	if ( panel.GetScriptName() != EXPLOSIVE_HOLD_PANEL_SCRIPTNAME )
		return

	AddCallback_OnUseEntity_ClientServer( panel, ExplosiveHoldDoor_OnUse )
	SetCallback_CanUseEntityCallback( panel, ExplosiveHoldDoor_CanUse )
	AddEntityCallback_GetUseEntOverrideText( panel, GetExplosiveHoldUseTextOverride )
}





























bool function ExplosiveHold_IsAnimatedInteraction( entity player, entity panel )
{
	vector playerToPanel = panel.GetOrigin() - player.GetOrigin()

	
	if ( DotProduct( panel.GetForwardVector(), playerToPanel ) < 0 )
		return true

	return false
}

bool function ExplosiveHoldDoor_CanUse( entity player, entity panel, int useFlags )
{
	if ( ExplosiveHold_IsAnimatedInteraction( player, panel ) && !SURVIVAL_PlayerCanUse_AnimatedInteraction( player, panel ) )
		return false

	vector playerToPanel = panel.GetOrigin() - player.GetOrigin()
	if ( DotProduct( panel.GetUpVector(), playerToPanel ) < -PANEL_USABLE_HEIGHT )
		return false

	return true
}


string function GetExplosiveHoldUseTextOverride( entity panel )
{
	entity player = GetLocalClientPlayer()

	if ( ExplosiveHold_IsAnimatedInteraction( player, panel ) )
	{
		if ( !ExplosiveHold_IsOpen( panel ) )
		{
			if ( !ExplosiveHold_PlayerHasGrenadeInInventory( player ) )
				return "#EXPLOSIVEHOLD_HINT_MISSING_GRENADE"
			else
				return "#EXPLOSIVEHOLD_HINT"
		}
		else
			return ""
	}

	return "#EXPLOSIVEHOLD_HINT_INTERIOR"
}


void function ExplosiveHoldDoor_OnUse( entity panel, entity player, int useInputFlags )
{
	if( player.IsInventoryOpen() )
		return

	if ( ExplosiveHold_IsAnimatedInteraction( player, panel ) )
	{

		if ( !ExplosiveHold_IsOpen( panel ) )
		{
			if ( IsBitFlagSet( useInputFlags, USE_INPUT_LONG ) )
			{
				if ( !ExplosiveHold_PlayerHasGrenadeInInventory( player ) )
				{



					return
				}
				thread ExplosiveHoldDoor_UseThink_Thread( panel, player )
			}
		}






	}




}

void function ExplosiveHoldDoor_UseThink_Thread( entity ent, entity playerUser )
{
	if( playerUser.IsInventoryOpen() )
		return

	ExtendedUseSettings settings
	settings.duration = 0.3








		settings.loopSound = "survival_titan_linking_loop"
		settings.successSound = "ui_menu_store_purchase_success"
		settings.icon = $""
		settings.hint = Localize( "#EXPLOSIVEHOLD_ACTIVATE" )
		settings.displayRui = $"ui/extended_use_hint.rpak"
		settings.displayRuiFunc = ExplosiveHoldDoor_DisplayRui


	waitthread ExtendedUse( ent, playerUser, settings )
}







































void function ExplosiveHoldDoor_Use_Failure( entity ent, entity playerUser, ExtendedUseSettings settings )
{






}

void function ExplosiveHoldDoor_DisplayRui( entity ent, entity player, var rui, ExtendedUseSettings settings )
{

		RuiSetString( rui, "holdButtonHint", settings.holdHint )
		RuiSetString( rui, "hintText", settings.hint )
		RuiSetGameTime( rui, "startTime", Time() )
		RuiSetGameTime( rui, "endTime", Time() + settings.duration )

}









































































































































































































































































































































































































































































































































































































































































bool function ExplosiveHold_PlayerHasGrenadeInInventory( entity player )
{
	foreach ( info in file.grenadeDatas )
	{
		int count = SURVIVAL_CountItemsInInventory( player, info.weaponName )
		if ( count > 0 )
			return true
	}

	return false
}

bool function ExplosiveHold_IsOpen( entity explosiveHoldEnt )
{
	
	if ( !IsValid( explosiveHoldEnt ) )
		return true

	entity explosiveHold = explosiveHoldEnt.GetParent()
	bool isOpen = false

	if ( IsValid( explosiveHold ) )
		isOpen = StatusEffect_HasSeverity( explosiveHold, eStatusEffect.hold_is_open )

	return isOpen
}


bool function ExplosiveHold_IsPlayerPlantingGrenade( entity player )
{
	entity possiblePanel = player.GetParent()
	if ( IsValid( possiblePanel ) && possiblePanel.HasKey( "script_name" ) && possiblePanel.GetScriptName() == EXPLOSIVE_HOLD_PANEL_SCRIPTNAME )
	{
		return true
	}

	return false
}


bool function ExplosiveHold_GetStartEmpty()
{
	return GetCurrentPlaylistVarBool( "explosivehold_start_open_and_empty", false )
}


entity function GetExplosiveHoldProxyForLoot( entity lootEnt )
{
	array< entity > explosiveHoldEnts = _ExplosiveHoldEnts_Get()
	foreach ( entity explosiveHold in explosiveHoldEnts )
	{
		if ( !IsValid( explosiveHold ) )
			continue

		foreach ( entity child in explosiveHold.GetLinkEntArray() )
		{
			if ( child.GetModelName() == EXPLOSIVE_HOLD_PROXY &&
				 sqrt( DistanceSqr( child.GetOrigin(), lootEnt.GetOrigin() ) ) < 2 * EXPLOSIVE_HOLD_MAX_LOOT_DISTANCE )
			{
				return child
			}
		}
	}

	return null
}























































































bool function ExplosiveHold_NoPlaceVols_Enabled()
{
	return( GetCurrentPlaylistVarBool( "explosivehold_noplacevols_enabled", false ) )
}

bool function ExplosiveHold_BreachTriggers_Enabled()
{
	return( GetCurrentPlaylistVarBool( "explosivehold_breachtriggers_enabled", true ) )
}



