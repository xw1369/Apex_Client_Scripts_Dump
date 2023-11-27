global function ShSniperTowers_Init

const string SNIPER_TOWER_PANEL = "sniper_tower_panel"














struct SniperTowerData
{
	entity panel









}

struct
{
	table< entity, SniperTowerData >		sniperTowerDataMap
} file

void function ShSniperTowers_Init()
{






		AddCreateCallback( "prop_dynamic", OnPanelCreated )

}




























































void function OnPanelCreated( entity panel )
{
	if ( panel.GetScriptName() != SNIPER_TOWER_PANEL )
		return

	AddCallback_OnUseEntity_ClientServer( panel, SniperTower_OnUse )
	SetCallback_CanUseEntityCallback( panel, SniperTower_CanUse )
	AddEntityCallback_GetUseEntOverrideText( panel, SniperTowerUseTextOverride )
}

string function SniperTowerUseTextOverride( entity panel )
{
	return "#SNIPERTOWER_HINT"
}


bool function SniperTower_CanUse( entity player, entity panel, int useFlags )
{
	if ( !SURVIVAL_PlayerCanUse_AnimatedInteraction( player, panel ) )
		return false

	return true
}

void function SniperTower_OnUse( entity panel, entity player, int useInputFlags )
{
	if ( IsBitFlagSet( useInputFlags, USE_INPUT_LONG ) )
		thread SniperTower_UseThink_Thread( panel, player )
}

void function SniperTower_UseThink_Thread( entity ent, entity playerUser )
{
	ent.EndSignal( "OnDestroy" )
	playerUser.EndSignal( "OnDeath" )

	ExtendedUseSettings settings
	settings.duration = 0.3






		settings.loopSound = "survival_titan_linking_loop"
		settings.successSound = "ui_menu_store_purchase_success"
		settings.icon = $""
		settings.hint = Localize( "#SNIPERTOWER_ACTIVATE" )
		settings.displayRui = $"ui/extended_use_hint.rpak"
		settings.displayRuiFunc = SniperTower_DisplayRui


	waitthread ExtendedUse( ent, playerUser, settings )
}


void function SniperTower_DisplayRui( entity ent, entity player, var rui, ExtendedUseSettings settings )
{
	RuiSetString( rui, "holdButtonHint", settings.holdHint )
	RuiSetString( rui, "hintText", settings.hint )
	RuiSetGameTime( rui, "startTime", Time() )
	RuiSetGameTime( rui, "endTime", Time() + settings.duration )
}



























