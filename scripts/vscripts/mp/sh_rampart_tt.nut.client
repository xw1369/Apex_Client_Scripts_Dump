global function Rampart_TT_Init
global function Rampart_TT_RegisterNetworking
global function IsRampartTTPanelLocked
global function GetRampartTTPanelForLoot
global function CheckRampartTTMuralLegends








global function ServertoClientCallback_VendingMachineTimerDone
global function ServertoClientCallback_PlayerUsedVendingMachine
global function ServertoClientCallback_VendingMachineInUse
global function ServertoClientCallback_RampartTT_SetCustomSpeakerIdx
global function ServertoClientCallback_RampartTT_BroadcastSystemPlay



global const string VEND_PANEL = "rampart_tt_vend_panel"
const string VEND_WEAPON_TARGET = "rampart_tt_vend_weapon_target"
const string VEND_SHIELD = "rampart_tt_vend_shield"
const string VEND_BLOCKER_VOLUME = "rampart_tt_vend_blocker"
const string VEND_SHIELD_AMBGENERIC = "rampart_tt_vend_shield_ambGen"
const string VEND_INST_BLUE = "rampart_tt_vend_blue"
const string VEND_INST_PURPLE = "rampart_tt_vend_purple"
const string VEND_INST_GOLD = "rampart_tt_vend_gold"
const string VEND_LOOT_TABLE_BLUE = "rampart_tt_blue_weapons"
const string VEND_LOOT_TABLE_PURPLE = "rampart_tt_purple_weapons"
const string VEND_LOOT_TABLE_GOLD = "rampart_tt_gold_weapons"
global const string VEND_SPAWNED_WEAPON =  "rampart_tt_vend_spawnedWeapon"
const string VEND_SPAWNED_AMMO =  "rampart_tt_vend_spawnedAmmo"
const float DISTANCE_PANEL_TO_LOOT_SQR = 3500
const string ALARM_SFX_POSITION = "rampart_tt_alarm_sfx_pos"
const string ALARM_VFX_POSITION = "rampart_tt_alarm_vfx_pos"
const float VEND_PICKUP_GRACE_PERIOD = 5.0


const string SFX_ALARM = "Loba_Ultimate_Staff_VaultAlarm"
const string SFX_PANEL_DENY = "menu_deny"
const string SFX_PANEL_SUCCESS = "ui_menu_store_purchase_success"
const string SFX_PANEL_LOOP = "survival_titan_linking_loop"
const string SFX_PANEL_SPEAKER = "diag_mp_rampart_tt_vendMachine"
const string SFX_VEND_POWERDOWN = "VendingMachine_Shield_PowerDown"
const string SFX_VEND_SUSTAIN = "VendingMachine_Shield_Sustain"


const int RUI_TRACK_INDEX_CAPTURE_END_TIME = 0 
const int RUI_TRACK_INDEX_REQUIRED_TIME = 1 
const int RUI_TRACK_INDEX_ACTIVATOR_TEAM = 4 
const int RUI_TRACK_INDEX_COLOR = 0 
const vector BIGMAUDE_DISPENSER_CRAFTING_COLOR = < 165, 255, 243 > / 255.0
const asset BIGMAUDE_DISPENSER_CRAFTING_ICON_ASSET = $"rui/hud/gametype_icons/survival/item_bigmaude_timer"
const asset BIGMAUDE_DISPENSER_FILL_BG_ICON_ASSET = $"rui/hud/gametype_icons/survival/obj_background_bigmaude_crafting"
const asset BIGMAUDE_DISPENSER_FILL_ICON_ASSET = $"rui/hud/gametype_icons/survival/obj_background_bigmaude_crafting_fill"


const asset RAMPART_TT_CSV_DIALOGUE = $"datatable/dialogue/desertlands_rampart_tt_dialogue.rpak"
const float BIG_MAUDE_AUDIO_RADIUS = 7500.0
const string SFX_WELCOME = "diag_mp_rampart_tt_vendMachineWelcome_3p"
const string SFX_PANEL_PLAYER_ALREADY_PURCHASED = "diag_mp_rampart_tt_vendMachineWeaponClaimed_3p"
const string SFX_VEND_DONE = "VendingMachine_VendComplete_Shop"
const string SFX_VEND_DONE_POI_WIDE = "VendingMachine_VendComplete_Broadcast"
const array <string> SFX_RAMPART_VO_PURCHASE = [ "diag_mp_rampart_tt_vendMachine_07",
												"diag_mp_rampart_tt_vendMachine_08",
												"diag_mp_rampart_tt_vendMachine_09",
												"diag_mp_rampart_tt_vendMachine_10",
												"diag_mp_rampart_tt_vendMachine_11",
												"diag_mp_rampart_tt_vendMachine_12",
												"diag_mp_rampart_tt_vendMachine_13",
												"diag_mp_rampart_tt_vendMachine_14",
												"diag_mp_rampart_tt_vendMachine_15",
												"diag_mp_rampart_tt_vendMachine_19",
												"diag_mp_rampart_tt_vendMachine_20",
												"diag_mp_rampart_tt_vendMachine_21",
												"diag_mp_rampart_tt_vendMachine_22",
												"diag_mp_rampart_tt_vendMachine_23",
												"diag_mp_rampart_tt_vendMachine_24",
												"diag_mp_rampart_tt_vendMachine_25",
												"diag_mp_rampart_tt_vendMachine_26",
												"diag_mp_rampart_tt_vendMachine_27",
												"diag_mp_rampart_tt_vendMachine_28",
												"diag_mp_rampart_tt_vendMachine_29",
												"diag_mp_rampart_tt_vendMachine_30",
												"diag_mp_rampart_tt_vendMachine_31",
												"diag_mp_rampart_tt_vendMachine_32",
												"diag_mp_rampart_tt_vendMachine_33",
												"diag_mp_rampart_tt_vendMachine_34",
												"diag_mp_rampart_tt_vendMachine_35",
												"diag_mp_rampart_tt_vendMachine_36" ]


const asset VFX_VEND_COMPLETE = $"P_Maude_Vending_Done"

const string VFX_SHIELD_DISABLE = "P_rampart_vendit_shield_disable"
const string VFX_ALARM_LIGHT = "P_vault_door_alarm_oly_mu1_sm"
const string VFX_ALARM_LIGHT_NAME = "rampart_tt_vfx_alarm_light"
const string MODEL_VEND_SHIELD = "mdl/desertlands/rampart_tt_vendit_01_energyfield_01.rmdl"


const string RAMPART_LORE_DATAPAD = "rampart_tt_datapad"
const array <string> SFX_DATAPAD_BANG = [ "diag_mp_bangalore_rtt_a1_1_3p", "diag_mp_bangalore_rtt_a1_2_3p", "diag_mp_bangalore_rtt_a1_3_3p" ]
const array <string> SFX_DATAPAD_BLISK = ["diag_mp_blisk_rtt_a1_1_3p", "diag_mp_blisk_rtt_a1_2_3p" ]
const array <string> SFX_DATAPAD_FRANCIS = [ "diag_mp_francis_rtt_a1_1_3p", "diag_mp_francis_rtt_a1_2_3p" ]
const array <string> SFX_DATAPAD_GIBRALTAR = [ "diag_mp_gibraltar_rtt_a1_1_3p", "diag_mp_gibraltar_rtt_a1_2_3p" ]
const array <string> SFX_DATAPAD_MIRAGE = [ "diag_mp_mirage_rtt_a1_1_3p", "diag_mp_mirage_rtt_a1_2_3p", "diag_mp_mirage_rtt_a1_3_3p" ]
const array <string> SFX_DATAPAD_SEER = [ "diag_mp_seer_rtt_a1_1_3p","diag_mp_seer_rtt_a1_2_3p" ]
const array <string> SFX_DATAPAD_VALK = [ "diag_mp_valkyrie_rtt_a1_1_3p","diag_mp_valkyrie_rtt_a1_2_3p","diag_mp_valkyrie_rtt_a1_3_3p" ]

const string RAMPART_LORE_MENSIGN = "rampart_tt_lore_mensign"
const string RAMPART_LORE_SHOPSIGN = "rampart_tt_lore_shopsign"
const string RAMPART_LORE_PORTRAIT = "rampart_tt_lore_portrait"
const string RAMPART_LORE_SISTER = "rampart_tt_lore_sister"

const string SFX_LORE_MENSIGN = "bc_mirage_rrtReactMSign"
const string SFX_LORE_SHOPSIGN = "bc_rampart_rrtReactSign"
const string SFX_LORE_PORTRAIT = "bc_rampart_rrtReactPortrait"
const string SFX_LORE_SISTER = "bc_rampart_rrtReactSister"

const array <string> RAMPART_TT_S10_MURAL_LEGENDS = [ "character_bloodhound",
													  "character_gibraltar",
													  "character_bangalore",
													  "character_caustic",
													  "character_lifeline",
													  "character_mirage",
													  "character_pathfinder",
													  "character_wraith",
													  "character_octane",
													  "character_wattson",
													  "character_crypto",
													  "character_revenant",
													  "character_loba",
													  "character_rampart",
													  "character_horizon",
													  "character_fuse",
													  "character_valkyrie",
													  "character_seer" ]
struct
{
	entity alarm_sfxPos
	bool alarmActive = false
	float alarmTime
	array < array <string> > datapadDialogue = []

	float vendPickupGracePeriod

	table<entity, bool> isVending
	table<entity, bool> hasPurchased
	table<entity, bool> hasEntered
	bool speakerOnCooldown
	bool welcomeOnCooldown

	int 	customQueueIdx

	float 	rampartLineFinishedPlayingTime = -1.0
	array<entity> customSpeakers

}file

















void function Rampart_TT_Init()
{
	AddCallback_EntitiesDidLoad( EntitiesDidLoad )














		AddCreateCallback( "prop_dynamic", OnPanelCreated )
		AddCreateCallback( "prop_dynamic", OnLoreCreated )
		AddCreateCallback( PLAYER_WAYPOINT_CLASSNAME, SetupVendWaypoint )

}


void function EntitiesDidLoad()
{
	if ( !IsRampartTTEnabled() )
		return

	RegisterCSVDialogue( RAMPART_TT_CSV_DIALOGUE )






















}


bool function IsRampartTTEnabled()
{
	if ( GetCurrentPlaylistVarBool( "rampart_tt_enabled", true ) )
	{
		return HasEntWithScriptName( VEND_PANEL )
	}

	return false
}



bool function RampartHasStock()
{
	return GetCurrentPlaylistVarBool( "rampart_tt_stocked", true )
}


void function Rampart_TT_RegisterNetworking()
{
	Remote_RegisterClientFunction( "ServertoClientCallback_VendingMachineTimerDone", "entity" )
	Remote_RegisterClientFunction( "ServertoClientCallback_PlayerUsedVendingMachine", "entity", "entity" )
	Remote_RegisterClientFunction( "ServertoClientCallback_VendingMachineInUse", "entity" )
	Remote_RegisterClientFunction( "ServertoClientCallback_RampartTT_SetCustomSpeakerIdx", "int", 0, NUM_TOTAL_DIALOGUE_QUEUES )
	Remote_RegisterClientFunction( "ServertoClientCallback_RampartTT_BroadcastSystemPlay", "int", -1, SFX_RAMPART_VO_PURCHASE.len() - 1, "bool" )
}




















































































































void function OnPanelCreated( entity panel )
{
	if ( panel.GetScriptName() != VEND_PANEL )
		return

	AddCallback_OnUseEntity_ClientServer( panel, Vend_OnUse )
	SetCallback_CanUseEntityCallback( panel, Vend_CanUse )
	AddEntityCallback_GetUseEntOverrideText( panel, Vend_UseTextOverride )
}


bool function Vend_CanUse( entity player, entity panel, int useFlags)
{
	if ( !SURVIVAL_PlayerCanUse_AnimatedInteraction( player, panel ) )
		return false

	return true
}


string function Vend_UseTextOverride( entity panel )
{
	entity player = GetLocalViewPlayer()
	if ( !IsValid (player) )
	{
		return ""
	}

	string str = Localize( "#RAMPART_TT_BUY", Vend_GetCost( panel ) )
	if ( Crafting_IsDispenserCraftingEnabled() )
	{
		CustomUsePrompt_Show( panel )
		CustomUsePrompt_SetSourcePos( panel.GetOrigin() + ( panel.GetScriptName() == VEND_PANEL ? < 0, 0, 30 > : <0,0,-20> ) )
		CustomUsePrompt_SetLineColor( BIGMAUDE_DISPENSER_CRAFTING_COLOR )

		CustomUsePrompt_SetText( "" )
		if ( panel in file.isVending )
		{
			CustomUsePrompt_SetText( Localize("#DISPENSERS_RAMPART_TT_VENDING") )
		}
		else if ( UseOneTimePurchaseVendingMachines() && ( player in file.hasPurchased ) )
		{
			CustomUsePrompt_SetText( Localize("#DISPENSERS_RAMPART_TT_PURCHASED") )
		}
		else
		{
			CustomUsePrompt_SetText( Localize("#DISPENSERS_RAMPART_TT_READY") )
		}
		if ( PlayerIsInADS( player ) )
			CustomUsePrompt_ShowSourcePos( false )
		else
			CustomUsePrompt_ShowSourcePos( true )

		return ""
	}
	return str
}



void function Vend_OnUse( entity panel, entity player, int useInputFlags )
{
	if( Vend_CostCheck( panel, player ) )
	{
		if ( IsBitFlagSet( useInputFlags, USE_INPUT_LONG ) )
		{
			thread Vend_UseThink_Thread( panel, player )
		}
	}
	else
	{



	}
}


void function Vend_UseThink_Thread( entity ent, entity playerUser )
{
	if ( Crafting_IsDispenserCraftingEnabled() )
	{
		if ( ent in file.isVending )
		{



			return

		}

		if ( UseOneTimePurchaseVendingMachines() && ( playerUser in file.hasPurchased ) )
		{








			return
		}
	}

	ExtendedUseSettings settings
	settings.duration = 0.3

	if ( Crafting_IsDispenserCraftingEnabled() )
		settings.duration = 0.66






	settings.icon = $""
	settings.hint = Localize( "#RAMPART_TT_BUYING" )

	if ( Crafting_IsDispenserCraftingEnabled() )
		settings.hint = Localize("#DISPENSERS_RAMPART_TT_UNLOCK")

	settings.displayRui = $"ui/extended_use_hint.rpak"
	settings.displayRuiFunc = Vend_DisplayRui
	settings.loopSound = SFX_PANEL_LOOP


	ent.EndSignal( "OnDestroy" )
	playerUser.EndSignal( "OnDeath" )

	waitthread ExtendedUse( ent, playerUser, settings )
}



void function Vend_DisplayRui( entity ent, entity player, var rui, ExtendedUseSettings settings )
{
	RuiSetString( rui, "holdButtonHint", settings.holdHint )
	RuiSetString( rui, "hintText", settings.hint )
	RuiSetGameTime( rui, "startTime", Time() )
	RuiSetGameTime( rui, "endTime", Time() + settings.duration )
}














































































































































































bool function Vend_CostCheck( entity panel, entity player )
{
	if( Crafting_IsEnabled() && Crafting_GetPlayerCraftingMaterials( player ) >= Vend_GetCost( panel ) )
	{
		return true
	}

	
	return false
}


int function Vend_GetCost( entity panel )
{
	string instName = panel.GetInstanceName()
	int value
	switch( instName )
	{
		case VEND_INST_BLUE:
		{
			value = 50
			if ( Crafting_IsDispenserCraftingEnabled() )
				value = 0
			break
		}
		case VEND_INST_PURPLE:
		{
			value = 75
			if ( Crafting_IsDispenserCraftingEnabled() )
				value = 0
			break
		}
		case VEND_INST_GOLD:
		{
			value = 100
			if ( Crafting_IsDispenserCraftingEnabled() )
				value = 0
			break
		}
	}
	return value
}


















bool function IsRampartTTPanelLocked ( entity vendPanel )
{
	return GradeFlagsHas( vendPanel, eGradeFlags.IS_LOCKED )
}


entity function GetRampartTTPanelForLoot( entity lootEnt )
{
	array<entity> linkedEnts = lootEnt.GetLinkParentArray()
	if ( linkedEnts.len() > 0 )
	{
		foreach (entity linkedEnt in linkedEnts)
		{
			if ( linkedEnt.GetScriptName() == VEND_PANEL )
			{
				return linkedEnt
			}
		}
	}

	





	vector lootLocation = lootEnt.GetOrigin()

	foreach (entity panel in GetEntArrayByScriptName( VEND_PANEL ))
	{
		foreach ( entity child in panel.GetLinkEntArray() )
		{
			if ( child.GetOrigin() == lootLocation )
				return panel
		}
	}

	return null
}











































































































































































































































bool function DataPad_CanUse( entity player, entity datapad, int useFlags)
{
	if ( !SURVIVAL_PlayerCanUse_AnimatedInteraction( player, datapad ) )
		return false

	return true
}








































void function OnLoreCreated( entity loreEnt )
{
	switch( loreEnt.GetScriptName() )
	{
		case RAMPART_LORE_MENSIGN:
		{
			AddEntityCallback_GetUseEntOverrideText( loreEnt, MirageOnly_UseTextOverride )
			break
		}
		case RAMPART_LORE_PORTRAIT:
		{
			AddEntityCallback_GetUseEntOverrideText( loreEnt, RampartOnly_UseTextOverride )
			break
		}
		case RAMPART_LORE_SISTER:
		{
			AddEntityCallback_GetUseEntOverrideText( loreEnt, RampartOnly_UseTextOverride )
			break
		}
		case RAMPART_LORE_SHOPSIGN:
		{
			AddEntityCallback_GetUseEntOverrideText( loreEnt, RampartOnly_UseTextOverride )
			break
		}
		default:
		{
			return
		}
	}
}























bool function Lore_CanUse( entity player, entity loreEnt, int useFlags)
{
	if ( !SURVIVAL_PlayerCanUse_AnimatedInteraction( player, loreEnt ) )
		return false

	return true
}


string function MirageOnly_UseTextOverride( entity loreEnt )
{
	entity player = GetLocalViewPlayer()
	if ( !IsValid (player) )
	{
		return ""
	}

	ItemFlavor character = LoadoutSlot_GetItemFlavor( ToEHI( player ), Loadout_Character() )
	string characterRef  = ItemFlavor_GetCharacterRef( character ).tolower()
	if ( characterRef != "character_mirage" )
	{
		return "#RAMPART_TT_REQ_MIRAGE"
	}

	return "#RAMPART_TT_LORE_INTERACT"
}



string function RampartOnly_UseTextOverride( entity loreEnt )
{
	entity player = GetLocalViewPlayer()
	if ( !IsValid (player) )
	{
		return ""
	}

	ItemFlavor character = LoadoutSlot_GetItemFlavor( ToEHI( player ), Loadout_Character() )
	string characterRef  = ItemFlavor_GetCharacterRef( character ).tolower()
	if ( characterRef != "character_rampart" )
	{
		return "#RAMPART_TT_REQ_RAMPART"
	}

	return "#RAMPART_TT_LORE_INTERACT"
}




























































bool function CheckRampartTTMuralLegends( entity player )
{
	
	ItemFlavor character = LoadoutSlot_GetItemFlavor( ToEHI( player ), Loadout_Character() )
	string playerChar  = ItemFlavor_GetCharacterRef( character ).tolower()
	foreach ( validChar in RAMPART_TT_S10_MURAL_LEGENDS )
	{
		if( validChar == playerChar )
		{
			return true
		}
	}

	
	return false
}

bool function UseOneTimePurchaseVendingMachines()
{
	return GetCurrentPlaylistVarBool( "rampart_tt_single_use_vend", true )
}

float function BluePaintballVendTime()
{
	return GetCurrentPlaylistVarFloat( "rampart_tt_blue_vend_time", 8.0 )
}

float function PurpPaintballVendTime()
{
	return GetCurrentPlaylistVarFloat( "rampart_tt_purp_vend_time", 12.0 )
}


void function ServertoClientCallback_VendingMachineTimerDone ( entity vend )
{
	if ( vend in file.isVending )
		file.isVending [ vend ] = false
}

void function ServertoClientCallback_PlayerUsedVendingMachine ( entity player, entity panel )
{
	file.hasPurchased [ player ] <- true
}

void function ServertoClientCallback_VendingMachineInUse ( entity panel )
{
	file.isVending [ panel ] <- true
}







































































































































































































void function SetupVendWaypoint( entity waypoint )
{
	if ( waypoint.GetWaypointType() != eWaypoint.OBJECTIVE || Waypoint_GetPingTypeForWaypoint( waypoint ) != ePingType.RAMPART_TT_VENDING )
	{
		return
	}

	thread Thread_SetupVendWaypoint_Internal( waypoint )
}

void function Thread_SetupVendWaypoint_Internal( entity waypoint )
{
	if ( waypoint.GetWaypointType() != eWaypoint.OBJECTIVE || Waypoint_GetPingTypeForWaypoint( waypoint ) != ePingType.RAMPART_TT_VENDING )
	{
		return
	}

	int timeoutCounter = 0
	while ( IsValid( waypoint ) && waypoint.wp.ruiHud == null )
	{
		printf( "RAMPART_TT: Waypoint WaitFrame" )
		WaitFrame()
		timeoutCounter++
		if (timeoutCounter > 1000)
			return
	}

	if ( !IsValid( waypoint ) )
	{
		printf( "RAMPART_TT: Waypoint Invalid" )
		return
	}

	RuiSetFloat( waypoint.wp.ruiHud, "maxDrawDistance", 3000 )
	RuiSetFloat( waypoint.wp.ruiHud, "iconSize", 72.0 )
	RuiSetFloat( waypoint.wp.ruiHud, "iconSizePinned", 72.0 )
	RuiSetImage( waypoint.wp.ruiHud, "outerIcon", BIGMAUDE_DISPENSER_CRAFTING_ICON_ASSET )
	RuiSetImage( waypoint.wp.ruiHud, "innerIcon", BIGMAUDE_DISPENSER_CRAFTING_ICON_ASSET )
	RuiSetImage( waypoint.wp.ruiHud, "fillBackgroundImage", BIGMAUDE_DISPENSER_FILL_BG_ICON_ASSET )
	RuiSetImage( waypoint.wp.ruiHud, "fillImage", BIGMAUDE_DISPENSER_FILL_ICON_ASSET )

	RuiSetInt( waypoint.wp.ruiHud, "yourObjectiveStatus", 2 )
	RuiSetInt( waypoint.wp.ruiHud, "yourTeamIndex", GetLocalViewPlayer().GetTeam() )
	RuiSetInt( waypoint.wp.ruiHud, "roundState", 0 )
	RuiSetString( waypoint.wp.ruiHud, "pingPrompt", Localize( "#RAMPART_TT_VENDING" ) )
	RuiSetString( waypoint.wp.ruiHud, "pingPromptForOwner", Localize( "#RAMPART_TT_VENDING" ) )
	RuiSetBool( waypoint.wp.ruiHud, "reverseProgress", false )
	RuiSetBool( waypoint.wp.ruiHud, "iconColorOverride", true )
	RuiSetFloat3( waypoint.wp.ruiHud, "iconColor", Rampart_TT_GetWaypointColor( waypoint ) )

	RuiTrackGameTime( waypoint.wp.ruiHud, "captureEndTime", waypoint, RUI_TRACK_WAYPOINT_GAMETIME, RUI_TRACK_INDEX_CAPTURE_END_TIME )
	RuiTrackFloat( waypoint.wp.ruiHud, "captureTimeRequired", waypoint, RUI_TRACK_WAYPOINT_FLOAT, RUI_TRACK_INDEX_REQUIRED_TIME )
	RuiTrackInt( waypoint.wp.ruiHud, "currentControllingTeam", waypoint, RUI_TRACK_WAYPOINT_INT, RUI_TRACK_INDEX_ACTIVATOR_TEAM )
}

vector function Rampart_TT_GetWaypointColor( entity waypoint )
{
	if ( Crafting_IsDispenserCraftingEnabled() )
		return SrgbToLinear( BIGMAUDE_DISPENSER_CRAFTING_COLOR )

	return ( GetKeyColor( COLORID_LOOT_TIER0 + waypoint.GetWaypointInt( 5 ) ) / 255.0 )
}

void function ServertoClientCallback_RampartTT_SetCustomSpeakerIdx( int customQueueIdx )
{
	file.customQueueIdx = customQueueIdx
	file.customSpeakers.extend( GetEntArrayByScriptName( "Rampart_TT_Speaker" ) )
	RegisterCustomDialogueQueueSpeakerEntities( customQueueIdx, GetEntArrayByScriptName( "Rampart_TT_Speaker" ) )
}

const float DIALOGUE_DEBOUNCE_TIME = 5.0
void function ServertoClientCallback_RampartTT_BroadcastSystemPlay( int idxToPlay, bool overlapAudio )
{
	if ( file.customSpeakers.len() < 1 )
		return

	if ( Time() < file.rampartLineFinishedPlayingTime )
		return

	string lineToPlay
	if ( idxToPlay == -1 )
	{
		lineToPlay =  SFX_VEND_DONE_POI_WIDE
	}
	else
	{
		lineToPlay = SFX_RAMPART_VO_PURCHASE[idxToPlay]
	}

	if ( !overlapAudio )
	{
		float duration = GetSoundDuration( GetAnyDialogueAliasFromName( lineToPlay ) )
		file.rampartLineFinishedPlayingTime = Time() + duration + DIALOGUE_DEBOUNCE_TIME
	}

	int dialogueFlags = eDialogueFlags.USE_CUSTOM_QUEUE | eDialogueFlags.USE_CUSTOM_SPEAKERS | eDialogueFlags.BLOCK_LOWER_PRIORITY_QUEUE_ITEMS
	PlayDialogueOnCustomSpeakers( GetAnyAliasIdForName( lineToPlay ), dialogueFlags, file.customQueueIdx )
}

