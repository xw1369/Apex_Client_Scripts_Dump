
global function GamemodeTdmShared_Init
global function ClGamemodeTdm_Init

void function GamemodeTdmShared_Init()
{
	SetScoreEventOverrideFunc( TDM_SetScoreEventOverride )


	SetGameModeSuddenDeathAnnouncementSubtext( "#GAMEMODE_ANNOUNCEMENT_SUDDEN_DEATH_TDM" )

}

void function TDM_SetScoreEventOverride()
{
	ScoreEvent_SetGameModeRelevant( GetScoreEvent( "KillPilot" ) )
}

void function ClGamemodeTdm_Init()
{

}
