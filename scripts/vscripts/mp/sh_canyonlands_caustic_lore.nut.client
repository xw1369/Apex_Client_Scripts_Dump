




global function ClCanyonLandsCausticLore_Init



const string PANEL_DENY_SFX = "Caustic_TT_Screen_Deny"

const string LAB_DOOR_SCRIPTNAME = "CausticLabDoor"
const string LAB_ACCESS_PANEL_SCRIPTNAME = "CausticLabAccess"

struct
{
	array< entity > labDoors
	array< entity > labAccessPanels
} file












void function ClCanyonLandsCausticLore_Init()
{
	AddCreateCallback( "prop_dynamic", LabAccessPanelSpawned )
	AddCreateCallback( "prop_door", LabDoorSpawned )
}



void function LabAccessPanelSpawned( entity panel )
{
	if ( !IsValidLabAccessPanelEnt( panel ) )
		return

	file.labAccessPanels.append( panel )





	SetLabAccessPanelUsable( panel )
}

void function SetLabAccessPanelUsable ( entity panel )
{








		AddCallback_OnUseEntity_ClientServer( panel, OnLabAccessPanelUse )
		AddEntityCallback_GetUseEntOverrideText( panel, OnLabAccessTextOverride )

}

void function OnLabAccessPanelUse( entity panel, entity playerUser, int useInputFlags )
{
	ItemFlavor character = LoadoutSlot_GetItemFlavor( ToEHI( playerUser ), Loadout_Character() )
	string characterRef  = ItemFlavor_GetCharacterRef( character ).tolower()
	if ( characterRef != "character_caustic" && characterRef != "character_wattson" )
	{

			EmitSoundOnEntity( panel, PANEL_DENY_SFX )

		return
	}



		EmitSoundOnEntity( panel, "lootVault_Access" )














}


string function OnLabAccessTextOverride( entity ent )
{
	entity player = GetLocalViewPlayer()
	if ( !IsValid (player) )
	{
		return ""
	}

	ItemFlavor character = LoadoutSlot_GetItemFlavor( ToEHI( player ), Loadout_Character() )
	string characterRef  = ItemFlavor_GetCharacterRef( character ).tolower()
	if ( characterRef != "character_caustic" && characterRef != "character_wattson" )
	{
		return "#CAUSTIC_LAB_ACCESS_REQUIREMENT"
	}

	return "#CAUSTIC_LAB_ACCESS_USE"
}


bool function IsValidLabAccessPanelEnt ( entity ent )
{
	if ( ent.GetScriptName() == LAB_ACCESS_PANEL_SCRIPTNAME )
		return true

	return false
}

void function LabDoorSpawned( entity door )
{
	if ( !IsValidLabDoorEnt( door ) )
		return










	file.labDoors.append( door )
}

bool function IsValidLabDoorEnt( entity ent )
{
	if ( !IsDoor( ent ) )
		return false

	string scriptName = ent.GetScriptName()
	if ( scriptName != LAB_DOOR_SCRIPTNAME )
		return false

	return true
}