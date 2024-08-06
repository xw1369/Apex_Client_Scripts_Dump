




global function ClCommonStoryEvents_Init









global function GetTelTeasePhase

const asset BLIP_MODEL_1 = $"mdl/vistas/olympus_blip1.rmdl"
const asset BLIP_MODEL_2 = $"mdl/vistas/olympus_blip2.rmdl"
const asset BLIP_MODEL_3 = $"mdl/vistas/olympus_blip3.rmdl"

global enum eTeleportPhase
{
	DISABLED,
	PHASE_1,
	PHASE_2,
	PHASE_3,

	_count
}



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

































int function GetTelTeasePhase()
{
#if DEV
		int overridePhase =	GetCurrentPlaylistVarInt( "dev_tel_tease_phase", -1 )
		if ( overridePhase >= 0 && overridePhase < eTeleportPhase._count )
			return overridePhase
#endif

	int unixTimeNow = GetUnixTimestamp()
	if ( unixTimeNow > expect int( GetCurrentPlaylistVarTimestamp( "tel_p3", UNIX_TIME_FALLBACK_2038 ) ) )
	{
		return eTeleportPhase.PHASE_3
	}
	else if ( unixTimeNow > expect int( GetCurrentPlaylistVarTimestamp( "tel_p2", UNIX_TIME_FALLBACK_2038 ) ) )
	{
		return eTeleportPhase.PHASE_2
	}
	else if ( unixTimeNow > expect int( GetCurrentPlaylistVarTimestamp( "tel_p1", UNIX_TIME_FALLBACK_2038 ) ) )
	{
		return eTeleportPhase.PHASE_1
	}

	return eTeleportPhase.DISABLED
}

























































































      