




global function ClCommonStoryEvents_Init














struct
{

} file




























































































void function ClCommonStoryEvents_Init()
{
	AddCallback_EntitiesDidLoad( EntitiesDidLoad )


	if ( IsNightMap() && GetCurrentPlaylistVarBool( "EnableBWScreen", true ))
	{
		AddCallback_GameStateEnter( eGameState.WaitingForPlayers, OnWaitingForPlayers_Client )
		AddCallback_GameStateEnter( eGameState.PickLoadout, OnPickLoadout_Client )
		AddCallback_OnPlayerDisconnected( OnPlayerDisconnected )
	}


}



void function EntitiesDidLoad()
{

		if ( IsNightMap() && GetCurrentPlaylistVarBool( "UseNocAnnouncerAtNight", true ) )
		{
			SurvivalCommentary_SetHost( eSurvivalHostType.NOC )
		}

}
















void function OnWaitingForPlayers_Client()
{











	EmitUISound( "NightMaps_Intro_Static" )
	GfxDesaturateOn()

}

void function OnPickLoadout_Client()
{

	StopUISoundByName( "NightMaps_Intro_Static" )
	GfxDesaturateOff()

}


void function OnPlayerDisconnected( entity player )
{
	StopUISoundByName( "NightMaps_Intro_Static" )
}

      