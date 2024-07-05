global function InitPrivateMatchGameStatusMenu

















global function ServerCallback_PrivateMatch_OnEntityKilled
global function PrivateMatch_PopulateGameStatusMenu
global function PrivateMatch_GameStatus_GetPlayerButton
global function PrivateMatch_UpdateChatTarget
global function PrivateMatch_CycleAdminChatMode
global function PrivateMatch_ToggleAdminSpectatorChat
global function PrivateMatch_ToggleUpdatePlayerConnections
global function PrivateMatch_SquadEliminated
global function ClientCodeCallback_SpectatorGetOrderedTarget


const int TEAM_COUNT_PANEL_ONE 		= 2
const int TEAM_COUNT_PANEL_TWO 		= 7
const int TEAM_COUNT_PANEL_THREE 	= 11

const int ROSTER_LIST_SIZE = 20 

enum ePlayerHealthStatus
{
	PM_PLAYERSTATE_ALIVE,
	PM_PLAYERSTATE_BLEEDOUT,
	PM_PLAYERSTATE_DEAD,
	PM_PLAYERSTATE_REVIVING,
	PM_PLAYERSTATE_ELIMINATED,

	_count
}

enum eGameStatusPanel
{
	PM_GAMEPANEL_ROSTER,
	PM_GAMEPANEL_INGAME_SUMMARY,
	PM_GAMEPANEL_POSTGAME_SUMMARY,
	PM_GAMEPANEL_ADMIN,

	__count
}

struct TeamRosterStruct
{
	var           framePanel
	var           headerPanel
	var           listPanel
	table< var, int > connectionMap
	table< var, entity > buttonPlayerMap
}

struct TeamDetailsData
{
	int teamIndex
	int teamValue
	int teamKills
	int teamPlacement
	int playerAlive
	string teamName
}

struct PlayerData
{
	entity playerEntity
	asset characterPortrait 	= $"white"
	string playerName 			= ""
	int killCount 				= 0
}

struct TeamData
{
	array<PlayerData> players
	int placement = 0
}

struct AdminConfigData
{
	int chatMode
	bool spectatorChat
}

struct
{
	var menu

	var postGameSummaryPanel
	var inGameSummaryPanel
	var adminPanel

	var textChat
	var chatInputLine
	var chatButtonIcon

	var endMatchButton
	var chatModeButton
	var chatSpectCheckBox
	var chatTargetText

	bool enableMenu = false
	bool disableNavigateBack = false

	array< TeamRosterStruct > teamRosters
	array< int > teamIndices
	int lastPlayerCount = 0
	array< var > teamOverviewPanels

	int displayDirtyBit = 0
	table< int, TeamData > teamData
	table< string, PlayerData > playerData

	bool isInitialized = false
	bool tabsInitialized = false
	bool updateConnections = false

	bool inputsRegistered = false

	int maxTeamSize = MAX_TEAM_SIZE
	int overviewSizeTotal = 0
	AdminConfigData adminConfig
} file

void function InitPrivateMatchGameStatusMenu( var menu )
{
	file.menu = menu
	file.inGameSummaryPanel = Hud_GetChild( menu, "PrivateMatchOverviewPanel" )
	file.postGameSummaryPanel = Hud_GetChild( menu, "PrivateMatchSummaryPanel" )
	file.adminPanel = Hud_GetChild( menu, "PrivateMatchAdminPanel" )























		file.teamRosters.clear()
		int maxTeams = PrivateMatch_GetMaxTeamsForSelectedGamemode()
		if ( IsPrivateMatch() || IsPrivateMatchLobby() )
		{
			file.teamOverviewPanels.clear()

			file.overviewSizeTotal = 0
			SetupOverviewPanel( 1, TEAM_COUNT_PANEL_ONE, maxTeams )
			SetupOverviewPanel( 2, TEAM_COUNT_PANEL_TWO, maxTeams )
			SetupOverviewPanel( 3, TEAM_COUNT_PANEL_THREE, maxTeams )

			
			if( GetGameState() >= eGameState.Playing )
			{
				RunUIScript( "EnablePrivateMatchGameStatusMenu", file.enableMenu )
			}

			if ( GetLocalClientPlayer().HasMatchAdminRole() )
			{
				file.adminConfig.chatMode = ACM_ALL_PLAYERS
				file.adminConfig.spectatorChat = true

				SwitchChatModeButtonText( ACM_ALL_PLAYERS )
				RunUIScript( "SetSpecatorChatModeState", true, true )

				
				Remote_ServerCallFunction( "ClientCallback_PrivateMatchSetAdminConfig", file.adminConfig.chatMode, file.adminConfig.spectatorChat )
			}

			if ( !file.isInitialized )
			{
				AddOnSpectatorTargetChangedCallback( OnSpectateTargetChanged )

				AddCallback_OnPlayerMatchStateChanged( OnPlayerMatchStateChanged )

				
				AddCallback_GameStateEnter( eGameState.Playing, OnGameStateEnter_Playing )
				AddCallback_GameStateEnter( eGameState.WinnerDetermined, OnGameStateEnter_WinnerDetermined )
				SwitchChatModeButtonText( file.adminConfig.chatMode )

				PrivateMatch_RestoreDefaultPanels() 
				PrivateMatch_ClearTeamPresence()

				file.isInitialized = true
			}
		}

}




























































































































































void function DirtyAll()
{
	file.displayDirtyBit = ~0
}


void function DirtyBit( int teamIdx )
{
	file.displayDirtyBit = file.displayDirtyBit | ( 1 << teamIdx )
}


void function DirtyPlayerBit( entity player )
{
	DirtyBit( player.GetTeam() )
}

void function PrivateMatch_SquadEliminated( int teamIdx, int placement )
{
	if ( teamIdx in file.teamData )
	{
		file.teamData[ teamIdx ].placement = placement
	}
	else
	{
		TeamData tData
		tData.placement = placement
		file.teamData[ teamIdx ] <- tData
		file.teamIndices.push( teamIdx )
		file.teamIndices.sort()
	}
	DirtyAll()
}

int function ClientCodeCallback_SpectatorGetOrderedTarget( int targetIndex, int teamIndex )
{
	if ( teamIndex in file.teamData )
	{
		array<PlayerData> tData = file.teamData[teamIndex].players
		if ( targetIndex < tData.len() )
		{
			PlayerData pData = tData[targetIndex]
			if ( IsValid( pData.playerEntity ) )
				return GetPlayerArrayOfTeam( pData.playerEntity.GetTeam() ).find( pData.playerEntity )
		}
	}

	return -1
}

void function SwitchChatModeButtonText( int chatMode )
{
	string chatModeText = ""
	switch ( chatMode )
	{
		case ACM_PLAYER:
		{
			chatModeText = Localize( "#TOURNAMENT_PLAYER_CHAT" ) 
		}
		break
		case ACM_TEAM:
		{
			chatModeText = Localize( "#TOURNAMENT_TEAM_CHAT" ) 
		}
		break
		case ACM_SPECTATORS:
		{
			chatModeText = Localize( "#TOURNAMENT_SPECTATOR_CHAT" ) 
		}
		break
		case ACM_ALL_PLAYERS:
		{
			chatModeText = Localize( "#TOURNAMENT_ALL_CHAT" ) 
		}
		break
	}

	RunUIScript( "SetChatModeButtonText", chatModeText )
}

void function SetupOverviewPanel( int panelNum, int panelSize, int maxTeams )
{
	var teamOverview = Hud_GetChild( file.postGameSummaryPanel, "TeamOverview0" + panelNum )

	if ( maxTeams <= file.overviewSizeTotal )
	{
		Hud_Hide( teamOverview )
		return
	}

	Hud_Show( teamOverview )
	file.overviewSizeTotal += panelSize
	Hud_InitGridButtons( teamOverview, panelSize )
	var scrollPanel = Hud_GetChild( teamOverview, "ScrollPanel" )

	for ( int i = 0; i < panelSize; i++ )
	{
		var panel = Hud_GetChild( scrollPanel, "GridButton" + i )
		HudElem_SetRuiArg( panel, "teamPosition", file.teamOverviewPanels.len() + 1 )
		file.teamOverviewPanels.insert( file.teamOverviewPanels.len(), panel )
	}
}

void function ServerCallback_PrivateMatch_OnEntityKilled( entity attacker, entity victim )
{
	bool attackerIsPlayer = IsValid( attacker ) && attacker.IsPlayer()
	bool victimIsPlayer = IsValid( victim ) && victim.IsPlayer()

	
	if ( !victimIsPlayer )
		return

	
	if ( attackerIsPlayer && ( attacker != victim ) )
	{
		string uid = attacker.GetUserID()
		if ( uid in file.playerData )
		{
			DirtyPlayerBit( attacker )
		}
	}

	DirtyPlayerBit( victim )
}


int function SortPlayers( PlayerData a, PlayerData b )
{
	return a.playerName > b.playerName ? 1 : -1
}

int function SortTeams( TeamDetailsData a, TeamDetailsData b )
{
	if ( a.teamValue < b.teamValue )
		return 1

	if ( a.teamValue > b.teamValue )
		return -1

	return a.teamName > b.teamName ? 1 : -1
}

PlayerData function PrivateMatch_ExtractPlayerData( entity player )
{
	Assert( IsValid( player ) )

	PlayerData pData
	pData.playerEntity = player
	ItemFlavor character = LoadoutSlot_WaitForItemFlavor( ToEHI( player ), Loadout_Character() )
	pData.characterPortrait = CharacterClass_GetGalleryPortrait( character )
	pData.killCount = player.GetPlayerNetInt( "kills" )

		pData.playerName = player.GetPlayerNameWithClanTag()



	return pData
}

void function PrivateMatch_PopulateGameStatusMenu( var menu )
{
	if ( file.enableMenu == false )
		return

	DirtyAll() 
	thread UpdateTeamOverviewMenus() 
}

void function PrivateMatch_RestoreDefaultPanels()
{
	Hud_Hide( Hud_GetChild( file.inGameSummaryPanel, "EliminatedSquadsHeader" ) )

	{
		var overviewPanel  = Hud_GetChild( file.inGameSummaryPanel, "TeamOverview" )
		var scrollingPanel = Hud_GetChild( overviewPanel, "ScrollPanel" )
		for ( int i = 0; i < file.maxTeamSize + 2; ++i )
		{
			var button = Hud_GetChild( scrollingPanel, "GridButton" + i )
			HudElem_SetRuiArg( button, "teamPosition", 0 )
			Hud_Hide( button )
		}
	}

	{
		foreach ( panel in file.teamOverviewPanels )
		{
			Hud_Hide( panel )
		}
	}

	{
		foreach ( teamRoster in file.teamRosters )
		{
			Hud_Hide( teamRoster.listPanel )
			Hud_Hide( teamRoster.headerPanel )
			Hud_Hide( teamRoster.framePanel )
		}
	}

	DirtyAll()
}

void function PrivateMatch_ClearTeamPresence()
{
	file.teamIndices.clear()
	file.teamData.clear()
	file.playerData.clear()
	DirtyAll()
}

void function PrivateMatch_UpdateTeamPresence()
{
	foreach( player in GetPlayerArray() )
	{
		if ( !IsValid( player ) )
			continue

		if ( player.IsObserver() )
			continue

		PlayerData pData
		if ( player.GetUserID() in file.playerData )
		{
			pData = file.playerData[ player.GetUserID() ]
			pData.playerEntity = player
		}
		else
		{
			pData = PrivateMatch_ExtractPlayerData( player )
			file.playerData[ player.GetUserID() ] <- pData
		}

		TeamData tData
		if ( player.GetTeam() in file.teamData )
			tData = file.teamData[ player.GetTeam() ]
		else
		{
			file.teamData[ player.GetTeam() ] <- tData
			file.teamIndices.push( player.GetTeam() )
			file.teamIndices.sort()
			DirtyAll()
		}

		if ( !tData.players.contains( pData ) )
		{
			tData.players.append( pData )
			tData.players.sort( SortPlayers )
			DirtyPlayerBit( player )
		}
	}
}

void function PrivateMatch_GameStatus_Update( int dirtyBit )
{
	if ( dirtyBit == 0 )
		return

	array< TeamDetailsData > teamOrderArray
	foreach ( teamIndex, tData in file.teamData )
	{
		TeamDetailsData tDetails
		tDetails.teamIndex = teamIndex
		tDetails.teamName = PrivateMatch_GetTeamName( teamIndex )
		tDetails.teamPlacement = tData.placement
		tDetails.playerAlive = 0
		tDetails.teamKills = 0

		foreach ( pData in tData.players )
		{
			if( IsValid( pData.playerEntity ) )
			{
				if ( tData.placement == 0 && IsAlive( pData.playerEntity ) )
					tDetails.playerAlive++

				pData.killCount = pData.playerEntity.GetPlayerNetInt( "kills" )
			}

			tDetails.teamKills += pData.killCount
		}

		tDetails.teamValue = ( tDetails.playerAlive * 1000 ) + tDetails.teamKills - ( tDetails.teamPlacement * 100000 )
		teamOrderArray.append( tDetails )
	}

	teamOrderArray.sort( SortTeams )

	PrivateMatch_GameStatus_MidGame_Configure( teamOrderArray )
	PrivateMatch_GameStatus_PostGame_Configure( teamOrderArray )
}

void function PrivateMatch_GameStatus_MidGame_Configure( array<TeamDetailsData> teamOrderArray )
{
	foreach ( int i, panel in file.teamOverviewPanels )
	{
		if ( i < teamOrderArray.len() )
			thread PrivateMatch_GameStatus_TeamDetails_Configure( panel, teamOrderArray[i], true )
		else
			Hud_Hide( panel )
	}
}

void function PrivateMatch_GameStatus_PostGame_Configure( array<TeamDetailsData> teamOrderArray )
{
	
	
	int[22] gridIdxBinding = [
		0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20,
		1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21
	]

	bool showElimHeader = false
	var overviewPanel = Hud_GetChild( file.inGameSummaryPanel, "TeamOverview" )
	var scrollPanel = Hud_GetChild( overviewPanel, "ScrollPanel" )
	for ( int i = 0; i < ROSTER_LIST_SIZE; ++i )
	{
		int buttonIdx = showElimHeader ? gridIdxBinding[i + 2] : gridIdxBinding[i + 1]
		if ( i < teamOrderArray.len() )
		{
			TeamDetailsData team = teamOrderArray[i]
			if ( !showElimHeader && team.playerAlive == 0 )
			{
				PrivateMatch_GameStatus_ElimHeader_Configure( buttonIdx, scrollPanel )
				showElimHeader = true
				buttonIdx      = gridIdxBinding[i + 2]
			}

			var button = Hud_GetChild( scrollPanel, "GridButton" + buttonIdx )
			HudElem_SetRuiArg( button, "teamPosition", teamOrderArray[i].teamPlacement )
			thread PrivateMatch_GameStatus_TeamDetails_Configure( button, teamOrderArray[i] )
		}
		else
		{
			Hud_Hide( Hud_GetChild( scrollPanel, "GridButton" + buttonIdx ) )
		}
	}

	if ( !showElimHeader )
		Hud_Hide( Hud_GetChild( scrollPanel, "GridButton21" ) )
}

void function PrivateMatch_GameStatus_ElimHeader_Configure ( int idxLocation, var buttonPanel )
{
	var elimHeader = Hud_GetChild( file.inGameSummaryPanel, "EliminatedSquadsHeader" )
	Hud_Show( elimHeader )

	var button = Hud_GetChild( buttonPanel, "GridButton" + idxLocation )
	Hud_Hide( button )

	UIPos pos1 = REPLACEHud_GetAbsPos( button )
	UIPos pos2 = REPLACEHud_GetAbsPos( file.inGameSummaryPanel )
	Hud_SetPos( elimHeader, pos2.x - pos1.x, pos2.y - pos1.y )
}

void function PrivateMatch_GameStatus_TeamDetails_Configure( var panel, TeamDetailsData tData, bool postGame = false )
{
	array<PlayerData> teamPlayers = file.teamData[ tData.teamIndex ].players
	Hud_Show( panel )

	var panelRui = Hud_GetRui( panel )
	RuiSetString( panelRui, "teamName", tData.teamName )
	RuiSetInt( panelRui, "playersAlive", tData.playerAlive )
	RuiSetInt( panelRui, "kills", tData.teamKills )

	int idx = 0
	foreach ( int i, pData in teamPlayers )
	{
		if ( i < file.maxTeamSize )
		{
			RuiSetImage( panelRui, "playerImage" + i, pData.characterPortrait )
			RuiSetBool( panelRui, "playerAlive" + i, postGame || ( IsValid( pData.playerEntity ) && IsAlive( pData.playerEntity ) ) )
			idx = i
		}
	}

	for ( ++idx; idx < file.maxTeamSize; ++idx )
	{
		RuiSetImage( panelRui, "playerImage" + idx, $"" )
		RuiSetBool( panelRui, "playerAlive" + idx, false )
	}
}

void function PrivateMatch_GameStatus_ConfigurePlayerButton( var button, PlayerData pData )
{
	var buttonRui = Hud_GetRui( button )
	if ( IsValid( pData.playerEntity ) )
	{
		RuiSetInt( buttonRui, "playerState", GetPlayerHealthStatus( pData.playerEntity ) )
		RuiSetBool( buttonRui, "isObserveTarget", GetLocalClientPlayer().GetObserverTarget() == pData.playerEntity )
	}
	else
	{
		RuiSetInt( buttonRui, "playerState", ePlayerHealthStatus.PM_PLAYERSTATE_ELIMINATED )
		RuiSetBool( buttonRui, "isObserveTarget", false )
	}

	RuiSetString( buttonRui, "buttonText",  pData.playerName )
	RuiSetImage( buttonRui, "playerPortrait", pData.characterPortrait )
	RuiSetInt( buttonRui, "killCount", pData.killCount )
}

var function PrivateMatch_GameStatus_GetPlayerButton( entity player )
{
	foreach ( teamRoster in file.teamRosters )
	{
		foreach ( var button, entity buttonPlayer in teamRoster.buttonPlayerMap )
		{
			if ( !IsValid( button ) || !IsValid( buttonPlayer ) )
				continue

			if ( buttonPlayer == player )
			{
				return button
			}
		}
	}
}

void function OnRosterButton_Click( var button )
{
	entity observerTarget = null
	foreach ( teamRoster in file.teamRosters )
	{
		if ( button in teamRoster.buttonPlayerMap )
			observerTarget = teamRoster.buttonPlayerMap[ button ]
	}

	if ( !IsValid( observerTarget ) )
		return

	if ( !IsAlive( observerTarget ) )
		return

	if ( GetLocalClientPlayer().GetObserverTarget() == observerTarget )
		return

	Remote_ServerCallFunction( "ClientCallback_PrivateMatchChangeObserverTarget", observerTarget )
	RunUIScript( "ClosePrivateMatchGameStatusMenu", null )
}

void function RefreshEntityReference( entity player )
{
	if ( IsValid( player ) && player.IsPlayer() )
	{
		
		string uid = player.GetUserID()
		if ( uid in file.playerData )
			file.playerData[uid].playerEntity = player

		
		DirtyPlayerBit( player )
	}
}


void function OnSpectateTargetChanged( entity spectatingPlayer, entity oldSpectatorTarget, entity newSpectatorTarget )
{
	RefreshEntityReference( oldSpectatorTarget )
	RefreshEntityReference( newSpectatorTarget )
}

int function GetPlayerHealthStatus( entity player )
{
	int healthStatus = ePlayerHealthStatus.PM_PLAYERSTATE_ALIVE

	if ( !IsAlive( player ) )
	{
		int respawnStatus = GetRespawnStatus( player )
		switch ( respawnStatus )
		{
			case eRespawnStatus.PICKUP_DESTROYED:
			case eRespawnStatus.SQUAD_ELIMINATED:
				healthStatus = ePlayerHealthStatus.PM_PLAYERSTATE_ELIMINATED
				break

			case eRespawnStatus.WAITING_FOR_DELIVERY:
			case eRespawnStatus.WAITING_FOR_DROPPOD:
			case eRespawnStatus.WAITING_FOR_PICKUP:
			case eRespawnStatus.WAITING_FOR_RESPAWN:
				healthStatus = ePlayerHealthStatus.PM_PLAYERSTATE_DEAD
				break

			case eRespawnStatus.NONE:
			default:
				healthStatus = ePlayerHealthStatus.PM_PLAYERSTATE_ALIVE
				break
		}
	}

	if ( Bleedout_IsBleedingOut( player ) )
	{
		int bleedoutState = player.GetBleedoutState()

		switch ( bleedoutState )
		{
			case BS_BLEEDING_OUT:
			case BS_ENTERING_BLEEDOUT:
				healthStatus = ePlayerHealthStatus.PM_PLAYERSTATE_BLEEDOUT
				break

			case BS_EXITING_BLEEDOUT:
				healthStatus = ePlayerHealthStatus.PM_PLAYERSTATE_REVIVING
				break
		}
	}

	return healthStatus
}

void function OnGameStateEnter_Playing()
{
	DirtyAll() 
	TryEnablePrivateMatchGameStatusMenu()

	RunUIScript( "OnPrivateMatchStateChange", eGameState.Playing )
}

void function OnGameStateEnter_WinnerDetermined()
{
	RunUIScript( "OnPrivateMatchStateChange", eGameState.WinnerDetermined )
}

void function OnPlayerMatchStateChanged( entity player, int newState )
{
	if ( newState > ePlayerMatchState.SKYDIVE_PRELAUNCH )
	{
		DirtyAll() 
		TryEnablePrivateMatchGameStatusMenu()
	}
	else
	{
		PrivateMatch_RestoreDefaultPanels() 
		PrivateMatch_ClearTeamPresence()
	}
}

void function TryEnablePrivateMatchGameStatusMenu()
{
	if ( file.enableMenu )
		return

	if ( !IsPrivateMatch() )
		return

	if ( GetLocalClientPlayer().GetTeam() != TEAM_SPECTATOR )
		return

	if ( GetGameState() < eGameState.Playing )
		return

	RunUIScript( "EnablePrivateMatchGameStatusMenu", true )
	file.enableMenu = true

	PrivateMatch_PopulateGameStatusMenu( file.menu )
}

void function UpdateTeamOverviewMenus()
{
	while ( true )
	{
		PrivateMatch_UpdateTeamPresence()

		
		int dirtyBit = file.displayDirtyBit
		file.displayDirtyBit = 0

		PrivateMatch_GameStatus_Update( dirtyBit )

		wait 0.1
	}
}

void function PrivateMatch_UpdateChatTarget()
{
	int chatMode = file.adminConfig.chatMode

	entity observerTarget = GetLocalClientPlayer().GetObserverTarget()

	if( observerTarget == null )
	{
		RunUIScript( "SetChatTargetText", "" )
	}
	else if ( chatMode == ACM_PLAYER )
	{
		RunUIScript( "SetChatTargetText", observerTarget.GetPlayerName() )
	}
	else if ( chatMode == ACM_TEAM )
	{
		RunUIScript( "SetChatTargetText", PrivateMatch_GetTeamName( observerTarget.GetTeam() ) )
	}
	else
	{
		RunUIScript( "SetChatTargetText", "" )
	}

	Remote_ServerCallFunction( "ClientCallback_PrivateMatchSetAdminConfig", file.adminConfig.chatMode, file.adminConfig.spectatorChat )
}

void function PrivateMatch_CycleAdminChatMode()
{
	if ( !GetLocalClientPlayer().HasMatchAdminRole() )
		return

	if ( ++file.adminConfig.chatMode == ACM_COUNT )
		file.adminConfig.chatMode = 0

	SwitchChatModeButtonText( file.adminConfig.chatMode )
	PrivateMatch_UpdateChatTarget()

	RunUIScript( "SetSpecatorChatModeState", file.adminConfig.chatMode == ACM_ALL_PLAYERS, file.adminConfig.spectatorChat )
}

void function PrivateMatch_ToggleAdminSpectatorChat()
{
	if ( !GetLocalClientPlayer().HasMatchAdminRole() )
		return

	file.adminConfig.spectatorChat = !file.adminConfig.spectatorChat

	RunUIScript( "SetSpecatorChatModeState", file.adminConfig.chatMode == ACM_ALL_PLAYERS, file.adminConfig.spectatorChat )
	Remote_ServerCallFunction( "ClientCallback_PrivateMatchSetAdminConfig", file.adminConfig.chatMode, file.adminConfig.spectatorChat )
}

void function PrivateMatch_ToggleUpdatePlayerConnections( bool isActive )
{
	if ( isActive && file.updateConnections == false )
	{
		file.updateConnections = true
		thread UpdatePrivateMatchPlayerConnections()
	}
	else
	{
		file.updateConnections = false
	}
}

void function UpdatePrivateMatchPlayerConnections()
{
	while ( file.updateConnections )
	{
		foreach ( int idx, teamRoster in file.teamRosters )
		{
			foreach ( var button, entity buttonPlayer in teamRoster.buttonPlayerMap )
			{
				if ( IsValid( button ) )
				{
					int connectionQuality0 = ( button in teamRoster ) ? teamRoster.connectionMap[ button ] : -1
					int connectionQuality1 = IsValid( buttonPlayer ) ? buttonPlayer.GetConnectionQualityIndex() : 5

					if ( connectionQuality0 != connectionQuality1 )
					{
						teamRoster.connectionMap[ button ] <- connectionQuality1
						DirtyBit( TEAM_MULTITEAM_FIRST + idx )
					}
				}
			}
		}
		wait 1
	}
}



































































































































































































