




global function ClCommonStoryEvents_Init

















struct
{










		array < vector > blockBalloonCreationOrigins

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




void function ParseBalloonPlaylist()
{
	int numBalloonBlock = GetCurrentPlaylistVarInt( "balloon_block_count", 0 )

	for ( int index = 0; index < numBalloonBlock; index++ )
	{
		string playlistName = "balloon_block_count_" + index
		vector balloonBlockOrigin = StringToVector( GetCurrentPlaylistVarString( playlistName, "0 0 0" ) )
		file.blockBalloonCreationOrigins.append( balloonBlockOrigin )
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





















































































      