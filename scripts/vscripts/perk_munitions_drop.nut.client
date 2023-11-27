global function Perk_MunitionsDrop_Init


global function ServerToClient_DisplayMunitionsDropMessage





const float PERK_MUNITIONS_DROP_ICON_DRAW_DISTANCE = 3000

struct
{
	array<entity> respawnBeacons
} file

void function Perk_MunitionsDrop_Init()
{
	if ( GetCurrentPlaylistVarBool( "disable_perk_munitions_drop", true ) )
		return

	PerkInfo munitionsDrop
	munitionsDrop.perkId          = ePerkIndex.MUNITIONS_DROP

		munitionsDrop.activateCallback = OnActivate_PerkMunitionsDrop
		munitionsDrop.deactivateCallback = null

	Perks_RegisterClassPerk( munitionsDrop )

	Remote_RegisterClientFunction( "ServerToClient_DisplayMunitionsDropMessage" )
	RegisterSignal( "Update_TrackRespawnBeacons" )






}


void function OnActivate_PerkMunitionsDrop( entity player, string characterName )
{














}





































































































































































void function ServerToClient_DisplayMunitionsDropMessage()
{
	entity player = GetLocalViewPlayer()

	if ( !IsValid( player ) )
		return

	AddPlayerHint( 2.0, 0.25, $"", "#MUNITIONS_DROP_HINT" )
}

