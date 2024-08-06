

global function TreasureHunt_Init


global function ServerCallback_TreasureHunt_DisplayMessageToClient
global function TreasureHunt_InstanceObjectivePing



global function TreasureHunt_GetStarterPingFromTraceBlockerPing

















global const TREASUREHUNT_OBJECTIVE_SCRIPTNAME = "treasurehunt_objective"







































const int MAX_SUPPORTED_TEAMS = 4
const int INVALID_OBJ_INDEX_DEFAULT = -1
const int INVALID_OBJ_INDEX_OBJ_COMPLETE = -2


const asset CAPTURE_ZONE_INCOMING_VFX = $"P_lockdown_objective_incoming"
const asset CAPTURE_ZONE_START_VFX = $"P_lockdown_objective_start"
const asset CAPTURE_ZONE_END_VFX = $"P_lockdown_objective_end"
const asset CAPTURE_ZONE_REWARD_VFX = $"P_lockdown_objective_capture"
const asset CAPTURE_ZONE_REWARD_WEAPON_TRAIL_VFX = $"P_lockdown_objective_capture_trail"
const asset PLATFORM_MODEL = $"mdl/props/treasurehunt_platform_01/treasure_hunt_platform_01.rmdl"


const int CAPTURE_OBJ_WAYPOINT_FLOAT_IDX_START_TIME = 0
const int CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_INDEX = 3
const int CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_STATE = 4
const int CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_OWNER = 5

const float DELAYED_MESSAGE_DELAY = 2.5 
const float CAPTURE_OBJ_INCOMING_DURATION = 30.0 




const string LOCKDOWN_SFX_OBJ_CAPTURE_STARTED = "FreeDM_UI_InGame_FirstOnZone_1p"
const string LOCKDOWN_SFX_OBJ_CAPTURE_FINISHED = "FreeDM_UI_InGame_Zone_Capture_1p"
const string LOCKDOWN_SFX_OBJ_CAPTURING = "FreeDM_UI_InGame_Score_Gained_1P"
const string LOCKDOWN_SFX_CAPTURE_ZONE_ENTER = "FreeDM_UI_InGame_Zone_Enter_1p"
const string LOCKDOWN_SFX_CAPTURE_ZONE_EXIT = "FreeDM_UI_InGame_Zone_Exit_1p"
const string LOCKDOWN_SFX_CAPTURE_OBJ_INCOMING = "FreeDM_UI_InGame_Zone_Incoming_1P"
const string LOCKDOWN_SFX_CAPTURE_OBJ_SUDDEN_DEATH = "FreeDM_UI_InGame_Zone_SuddenDeath_1P"
const string LOCKDOWN_SFX_TEAM_HALFWAY_TO_WIN = "FreeDM_UI_InGame_MatchMidPoint_1P"
const string LOCKDOWN_SFX_TEAM_CLOSE_TO_WIN = "FreeDM_UI_InGame_MatchEndingSoon_1P"
const string LOCKDOWN_SFX_OBJECTIVE_CONTESTED = "FreeDM_UI_InGame_Zone_Contested_1P"
const string LOCKDOWN_SFX_TEAM_GAINED_LEAD = "FreeDM_UI_InGame_GainTheLead_1P"
const string LOCKDOWN_SFX_TEAM_LOST_LEAD = "FreeDM_UI_InGame_Demotion_1P"


const asset CAPTURE_OBJECTIVE_MAP_ICON = $"rui/hud/gametype_icons/control/capture_point_bg"


const string LOCKDOWN_VICTORY_SOUND = "FreeDM_UI_InGame_Victory_1P"
const string LOCKDOWN_LOSS_SOUND = "FreeDM_UI_InGame_Loss_1P"





























enum eTreasureHuntTimedEventType
{
	CAPTURE,
	_count
}

enum eTreasureHuntCaptureZoneState
{
	INCOMING,
	NEUTRAL,
	CAPTURING,
	CONTESTED,
	SUDDEN_DEATH,

	_count,
}

enum eTreasureHuntMessageIndex
{
	SCORE_CAPTURE_INITIATED,
	SCORE_CAPTURE_INITIATED_PERSONAL,
	SCORE_CAPTURING,
	SCORE_OBJECTIVE_CAPTURED,
	SCORE_OBJECTIVE_CAPTURED_PERSONAL,
	SCORE_OBJECTIVE_CAPTURED_SUDDENDEATH,
	SCORE_OBJECTIVE_CAPTURED_SUDDENDEATH_PERSONAL,
	SCORE_KILL,
	SCORE_KILL_FROM_ZONE,
	SCORE_KILL_WINNER,
	CAPTURE_OBJECTIVE_INCOMING,
	OBJECTIVE_ENTERS_CONTESTED_STATE,
	OBJECTIVE_ENTERS_SUDDEN_DEATH,
	TEAM_GAINED_LEAD,
	TEAM_LOST_LEAD,
	TEAM_HALFWAY_TO_WIN,
	TEAM_HALFWAY_TO_WIN_PERSONAL,
	TEAM_CLOSE_TO_WIN,
	TEAM_CLOSE_TO_WIN_PERSONAL,
	_count
}



















struct
{


















		var captureZoneStatusRui
		var lockdownScoreHud
		var lockdownScoreboardHud
		array < entity > objectiveWaypoints 
		array < var > objectiveWaypointRuis 

}file

void function TreasureHunt_Init()
{
	TreasureHunt_RegisterNetworking()


	PrecacheParticleSystem( CAPTURE_ZONE_INCOMING_VFX )
	PrecacheParticleSystem( CAPTURE_ZONE_START_VFX )
	PrecacheParticleSystem( CAPTURE_ZONE_END_VFX )
	PrecacheParticleSystem( CAPTURE_ZONE_REWARD_VFX )
	PrecacheParticleSystem( CAPTURE_ZONE_REWARD_WEAPON_TRAIL_VFX )
	TreasureHunt_RegisterTimedEvents()

	CaptureObjectivePing_AddCallback_SetGetCaptureObjectiveIDFromWaypointFunction( TreasureHunt_GetObjectiveIDFromWaypoint )
	CaptureObjectivePing_AddCallback_SetIsCaptureObjectivePingObjectiveWaypoint( GetIsWaypointTreasureHuntObjectiveWaypoint )





































		file.objectiveWaypoints.resize( GetMaxActiveObjectiveCount(), null )
		file.objectiveWaypointRuis.resize( GetMaxActiveObjectiveCount(), null )
		CaptureObjectivePing_AddCallback_SetIsCaptureObjectivePingCommsActionFunction( TreasureHunt_IsTreasureHuntObjectiveCommsAction )
		CaptureObjectivePing_AddCallback_SetGetObjectivesArrayFunction( TreasureHunt_GetObjectiveWaypointsArray )
		CaptureObjectivePing_AddCallback_OnObjectivePingCoundChanged( TreasureHunt_OnObjectiveWaypointPinged )
		FreeDM_SetDisplayScoreThread( TreasureHunt_DisplaySquadScore_Thread )
		FreeDM_SetScoreboardSetupFunc( TreasureHunt_ScoreboardSetup() )
		TreasureHunt_CreateCaptureZoneStatusRui()
		AddClientCallback_OnResolutionChanged( TreasureHunt_OnResolutionChanged )


	FreeDM_SetAudioEvent( eFreeDMAudioEvents.Victory_Sound, LOCKDOWN_VICTORY_SOUND )
	FreeDM_SetAudioEvent( eFreeDMAudioEvents.Defeat_Sound, LOCKDOWN_LOSS_SOUND )
}

void function TreasureHunt_RegisterNetworking()
{
	int pingCountMax = GetExpectedSquadSize() + 1
	Remote_RegisterClientFunction( "ServerCallback_TreasureHunt_DisplayMessageToClient", "int", 0, eTreasureHuntMessageIndex._count, "int", 0, GetScoreLimit_FromPlaylist() + 1 )
	RegisterNetworkedVariable( "treasureHunt_PlayerTimeOnObjectives", SNDC_PLAYER_GLOBAL, SNVT_TIME, 0.0 )
	RegisterNetworkedVariable( "treasureHunt_OccupiedObjectiveID", SNDC_PLAYER_GLOBAL, SNVT_INT, INVALID_OBJ_INDEX_DEFAULT )


		RegisterNetVarIntChangeCallback ( "treasureHunt_OccupiedObjectiveID", OnTreasureHuntOccupiedObjectiveIDChanged_Client )









}























float function GetTreasureHuntZoneHoldTime()
{






	return GetCurrentPlaylistVarFloat( "treasure_hunt_zone_hold_time", 20)
}



float function GetCaptureObjectiveSpawnDelayTime()
{






	return CAPTURE_OBJ_INCOMING_DURATION
}




















int function GetMaxActiveObjectiveCount()
{
	int nameCount = CaptureObjectivePing_GetObjectiveNameStringsArray().len()
	int playlistDefinedCount = GetCurrentPlaylistVarInt( "treasure_hunt_max_spawn_count", nameCount)

	Assert( playlistDefinedCount <= nameCount, "LOCKDOWN:  treasure_hunt_max_spawn_count playlist var tried to set the max number of active objectives to: " + playlistDefinedCount + " which is higher than the max number of objective names: " + nameCount + " the max will be set to " + nameCount )

	return minint( playlistDefinedCount, nameCount )
}























































































































































































































































































































































































































































































































































































bool function GetIsWaypointTreasureHuntObjectiveWaypoint( entity waypoint )
{
	return IsValid( waypoint ) && waypoint.GetNetworkedClassName() == PLAYER_WAYPOINT_CLASSNAME && waypoint.GetWaypointType() == eWaypoint.TREASUREHUNT_OBJECTIVE
}




bool function TreasureHunt_IsValidObjectiveIndex( int objectiveIndex )
{
	return objectiveIndex != INVALID_OBJ_INDEX_DEFAULT && objectiveIndex != INVALID_OBJ_INDEX_OBJ_COMPLETE
}









































































































































































































void function TreasureHunt_RegisterTimedEvents()
{
	TimedEventData objectiveData
	objectiveData.eventType = eTreasureHuntTimedEventType.CAPTURE
	objectiveData.shouldShowPreamble = false













		objectiveData.colorOverride = COLOR_WHITE
		objectiveData.eventName = "#EVENT_AIRDROP_NAME"
		objectiveData.eventDesc = "#EVENT_AIRDROP_DESC"


	TimedEvents_RegisterTimedEvent( objectiveData )
}































































































































































































































































































































































const vector ZONE_NEUTRAL_COLOR = <230, 230, 230> 
const vector ZONE_CONTESTED_COLOR = <247, 60, 82> 
const vector ZONE_SUDDENDEATH_COLOR = <247, 60, 82> 

















































































































































































































































































































































































void function OnTreasureHuntOccupiedObjectiveIDChanged_Client( entity player, int newOccupiedObjectiveID )
{
	
	if ( player == GetLocalClientPlayer() )
	{
		
		if ( TreasureHunt_IsValidObjectiveIndex( newOccupiedObjectiveID ) )
		{
			TreasureHunt_PlayCaptureZoneEnterExitSFX( true )
		}
		else if( newOccupiedObjectiveID == INVALID_OBJ_INDEX_DEFAULT )
		{
			TreasureHunt_PlayCaptureZoneEnterExitSFX( false )
		}
		
	}
}































































































































































void function TreasureHunt_InstanceObjectivePing( entity wp )
{
	thread TreasureHunt_CreateObjectiveIcon_Thread( wp )
}




const int OBJECTIVE_RUI_SORTING = 301
const int MIN_TEAM_INDEX_FOR_RUI = 0
const int MAX_TEAM_INDEX_FOR_RUI = 3
void function TreasureHunt_CreateObjectiveIcon_Thread( entity wp )
{
	Assert( IsNewThread(), "Must be threaded off" )

	FlagWait( "EntitiesDidLoad" )

	entity localViewPlayer = GetLocalViewPlayer()
	if ( !IsValid( localViewPlayer ) || !IsValid( wp ) )
		return

	localViewPlayer.EndSignal( "OnDestroy" )
	wp.EndSignal( "OnDestroy" )

	vector pos = wp.GetOrigin()
	vector iconColor = GetKeyColor( COLORID_HUD_LOOT_TIER5 ) * ( 1.0 / 255.0 )
	
	var minimapRui = Minimap_AddRuitPosition( pos, <0,90,0>, $"ui/minimap_square_object_lockdown_objective.rpak", 1.0, COLOR_WHITE )
	var fullmapRui = FullMap_AddRuiAtPos( pos, <0,90,0>, $"ui/in_world_minimap_square_lockdown_objective.rpak", 1.0, COLOR_WHITE )
	var objectiveRui = CreateWaypointRui( $"ui/waypoint_lockdown_capture_objective.rpak", OBJECTIVE_RUI_SORTING )
	int objectiveNameIndex = wp.GetWaypointInt( CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_INDEX )

	OnThreadEnd(
		function() : ( minimapRui, fullmapRui, objectiveRui, wp, objectiveNameIndex )
		{
			Minimap_CommonCleanup( minimapRui )
			Fullmap_RemoveRui( fullmapRui )
			RuiDestroy( fullmapRui )
			RuiDestroyIfAlive( objectiveRui )
			file.objectiveWaypoints[ objectiveNameIndex ] = null
			file.objectiveWaypointRuis[ objectiveNameIndex ] = null
		}
	)

	
	
	
	RuiSetFloat2( minimapRui, "iconScale", < 0.65, 0.65, 0 > )
	RuiSetString( minimapRui, "objectiveName", CaptureObjectivePing_GetObjectiveNameFromObjectiveID_Localized( objectiveNameIndex ) )
	RuiTrackInt( minimapRui, "currentOwner", wp, RUI_TRACK_WAYPOINT_INT, CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_OWNER )
	RuiTrackInt( minimapRui, "objectiveState", wp, RUI_TRACK_WAYPOINT_INT, CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_STATE )

	for ( int team = TEAM_IMC; team < TEAM_IMC + MAX_SUPPORTED_TEAMS; team++ )
	{
		int squadIndex = Squads_GetSquadUIIndex( team )
		RuiSetColorAlpha( minimapRui, "colorTeam" + squadIndex, TreasureHunt_GetTeamColor( team ), 1.0 )
	}

	
	
	
	RuiSetString( fullmapRui, "objectiveName", CaptureObjectivePing_GetObjectiveNameFromObjectiveID_Localized( objectiveNameIndex ) )
	RuiTrackInt( fullmapRui, "currentOwner", wp, RUI_TRACK_WAYPOINT_INT, CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_OWNER )
	RuiTrackInt( fullmapRui, "objectiveState", wp, RUI_TRACK_WAYPOINT_INT, CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_STATE )

	for ( int team = TEAM_IMC; team < TEAM_IMC + MAX_SUPPORTED_TEAMS; team++ )
	{
		int squadIndex = Squads_GetSquadUIIndex( team )
		RuiSetColorAlpha( fullmapRui, "colorTeam" + squadIndex, TreasureHunt_GetTeamColor( team ), 1.0 )
	}

	
	
	

	
	RuiKeepSortKeyUpdated( objectiveRui, true, "targetPos" )
	RuiSetFloat3( objectiveRui, "targetPos", wp.GetOrigin() )
	RuiTrackFloat3( objectiveRui, "playerAngles", localViewPlayer, RUI_TRACK_CAMANGLES_FOLLOW )
	RuiTrackFloat( objectiveRui, "objectiveStartTime", wp, RUI_TRACK_WAYPOINT_FLOAT, CAPTURE_OBJ_WAYPOINT_FLOAT_IDX_START_TIME )
	RuiTrackInt( objectiveRui, "objectiveState", wp, RUI_TRACK_WAYPOINT_INT, CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_STATE )
	RuiTrackInt( objectiveRui, "currentOwner", wp, RUI_TRACK_WAYPOINT_INT, CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_OWNER )
	RuiSetVisible( objectiveRui, true )
	RuiSetString( objectiveRui, "objectiveName", CaptureObjectivePing_GetObjectiveNameFromObjectiveID_Localized( objectiveNameIndex ) )

	
	file.objectiveWaypoints[ objectiveNameIndex ] = wp
	file.objectiveWaypointRuis[ objectiveNameIndex ] = objectiveRui

	
	RuiSetFloat( objectiveRui, "objectiveDuration", GetTreasureHuntZoneHoldTime() )
	RuiSetFloat( objectiveRui, "incomingDuration", GetCaptureObjectiveSpawnDelayTime() )

	
	for ( int team = TEAM_IMC; team < TEAM_IMC + MAX_SUPPORTED_TEAMS; team++ )
	{
		int squadIndex = Squads_GetSquadUIIndex( team )
		RuiSetColorAlpha( objectiveRui, "colorTeam" + squadIndex, TreasureHunt_GetTeamColor( team ), 1.0 )
		RuiSetAsset( objectiveRui, "iconTeam" + squadIndex, TreasureHunt_GetTeamIcon( team ) )
	}

	int yourTeamIndex = -1
	entity localPlayer = GetLocalClientPlayer()
	if ( IsValid( localPlayer ) )
		yourTeamIndex = Squads_GetSquadUIIndex( localPlayer.GetTeam() )
	RuiSetInt( objectiveRui, "yourTeamIndex", yourTeamIndex )

	
	RuiSetInt( objectiveRui, "friendliesOnObjective", 0 )
	RuiSetInt( objectiveRui, "enemiesOnObjective", 0 )

	WaitForever()
}



array < entity > function TreasureHunt_GetObjectiveWaypointsArray()
{
	return file.objectiveWaypoints
}




void function TreasureHunt_OnObjectiveWaypointPinged( entity objectiveWaypoint, int team, int count )
{
	if ( !IsValid( objectiveWaypoint ) || !file.objectiveWaypoints.contains( objectiveWaypoint ) )
		return

	entity localPlayer = GetLocalClientPlayer()

	if ( localPlayer.GetTeam() != team )
		return

	int objectiveIndex = TreasureHunt_GetObjectiveIDFromWaypoint( objectiveWaypoint )
	var rui = file.objectiveWaypointRuis[ objectiveIndex ]
	RuiSetInt( rui, "numTeamPings", count )
}













void function TreasureHunt_ScoreboardSetup()
{
	clGlobal.showScoreboardFunc = ShowScoreboardOrMap_Teams
	clGlobal.hideScoreboardFunc = HideScoreboardOrMap_Teams

	Teams_AddCallback_ScoreboardData( TreasureHunt_GetScoreboardData )
	Teams_AddCallback_Header( TreasureHunt_ScoreboardUpdateHeader )
	Teams_AddCallback_PlayerScores( TreasureHunt_GetPlayerScores )
	Teams_AddCallback_SortScoreboardPlayers( TreasureHunt_SortPlayersByScore )
	Teams_AddCallback_GetTeamColor( TreasureHunt_GetTeamColor )
	Teams_AddCallback_GetTeamName( TreasureHunt_GetTeamName )
	Teams_AddCallback_GetTeamIcon( TreasureHunt_GetTeamIcon )
	Teams_AddCallback_DoModeSpecificWork( TreasureHunt_ModeSpecificScoreboardWork, "LockdownScoreHud" )
}



ScoreboardData function TreasureHunt_GetScoreboardData()
{
	ScoreboardData data
	data.numScoreColumns = 2

	data.columnDisplayIcons.append( $"rui/hud/common/timer_icon" )
	data.columnDisplayIconsScale.append( 1.0 )
	data.columnNumDigits.append( 4 )

	data.columnDisplayIcons.append( $"rui/hud/gamestate/player_kills_icon" )
	data.columnDisplayIconsScale.append( 1.0 )
	data.columnNumDigits.append( 2 )

	return data
}



void function TreasureHunt_OnResolutionChanged()
{
	file.lockdownScoreboardHud = null
}



void function TreasureHunt_ModeSpecificScoreboardWork( var lockdownScoreElement )
{
	var lockdownScoreRui = Hud_GetRui( lockdownScoreElement )

	if ( file.lockdownScoreboardHud == null || !IsValid( file.lockdownScoreboardHud ) )
	{
		file.lockdownScoreboardHud = lockdownScoreRui
	}

	int scoreLimit = GetScoreLimit_FromPlaylist()
	RuiSetInt( lockdownScoreRui, "scoreLimit", scoreLimit )

	
	entity localPlayer = GetLocalClientPlayer()
	array< TeamScoreStruct > teamScores = []
	for ( int team = TEAM_IMC; team < TEAM_IMC + MAX_SUPPORTED_TEAMS; team++ )
	{
		int squadIndex = Squads_GetSquadUIIndex( team )

		array< entity > playersOnTeam = GetPlayerArrayOfTeam( team )
		bool isTeamVisible = true
		if ( playersOnTeam.len() <= 0 )
		{
			isTeamVisible = false
			RuiSetBool( lockdownScoreRui, "squadVisible" + squadIndex, false )
		}

		RuiSetBool( lockdownScoreRui, "yourTeam" + squadIndex, IsPlayerOnTeam( localPlayer, team ) )

		int teamScore = GameRules_GetTeamScore( team )
		RuiSetInt( lockdownScoreRui, "score_" + squadIndex, teamScore )

		TeamScoreStruct teamScoreData
		teamScoreData.teamNum = squadIndex
		teamScoreData.score = teamScore
		teamScoreData.isVisible = isTeamVisible
		teamScores.push( teamScoreData )

		if ( isTeamVisible )
			TreasureHunt_SetCharacterInfo( lockdownScoreRui, team, squadIndex )

		Hud_SetVisible( lockdownScoreElement, true )
	}

	
	teamScores.sort( TreasureHunt_OrderTeamsByScore )

	
	for ( int placement = 0; placement < MAX_SUPPORTED_TEAMS; placement++ )
	{
		int teamNum = teamScores[ placement ].teamNum
		RuiSetInt( lockdownScoreRui, "placementTeam" + teamNum, placement )
	}

	Hud_SetVisible( lockdownScoreElement, true )
	Hud_Show( lockdownScoreElement )
}



void function TreasureHunt_ScoreboardUpdateHeader( var headerRui, var frameRui, int team )
{
	if ( headerRui != null )
	{
		bool isWinning = team == GamemodeUtility_GetWinningTeamOrAlliance( true )
		RuiSetString( headerRui, "headerText", Localize( TreasureHunt_GetTeamName( team ) ) )
		RuiSetBool( headerRui, "isWinning", isWinning )
		RuiSetImage( headerRui, "teamIcon", TreasureHunt_GetTeamIcon(team))
		RuiSetInt( headerRui, "teamScore", GameRules_GetTeamScore( team ) )
		RuiSetInt( headerRui, "scoreLimit", GetScoreLimit_FromPlaylist() )
		RuiSetBool( headerRui, "useScoreLimitElements", true )
		RuiSetBool( headerRui, "useGunGameElements", false )
	}
}



array< string > function TreasureHunt_GetPlayerScores( entity player )
{
	array< string > scores
	int playerTeamOrAlliance = AllianceProximity_IsUsingAlliances() ? AllianceProximity_GetAllianceFromTeam( player.GetTeam() ) : player.GetTeam()

	string timeOnObjective
	DisplayTime dt = SecondsToDHMS( int( player.GetPlayerNetTime( "treasureHunt_PlayerTimeOnObjectives" ) ) )
	if ( dt.hours != 0 )
		timeOnObjective = format( "%d:%.2d:%.2d", dt.hours, dt.minutes, dt.seconds )
	else
		timeOnObjective = format( "%.2d:%.2d", dt.minutes, dt.seconds )

	scores.append( timeOnObjective )

	string eliminations = string( player.GetPlayerNetInt( "kills" ) )
	scores.append( eliminations )

	return scores
}




array< TeamsScoreboardPlayer > function TreasureHunt_SortPlayersByScore( array< TeamsScoreboardPlayer > players )
{
	players.sort( int function( TeamsScoreboardPlayer a, TeamsScoreboardPlayer b )
	{
		entity playerA = FromEHI( a.playerEHI )
		entity playerB = FromEHI( b.playerEHI )

		if( !IsValid( playerA ) || !IsValid( playerB ) )
			return 0

		float aTimeOnObjective = playerA.GetPlayerNetTime( "treasureHunt_PlayerTimeOnObjectives" )
		float bTimeOnObjective = playerB.GetPlayerNetTime( "treasureHunt_PlayerTimeOnObjectives" )
		int aKills = playerA.GetPlayerNetInt( "kills" )
		int bKills = playerB.GetPlayerNetInt( "kills" )

		if ( aTimeOnObjective > bTimeOnObjective ) return -1
		else if ( aTimeOnObjective < bTimeOnObjective ) return 1
		else
		{
			if ( aKills > bKills ) return -1
			else if ( aKills < bKills ) return 1
		}
		return 0
	}
	)

	return players
}



vector function TreasureHunt_GetTeamColor( int team )
{
	int squadIndex = Squads_GetSquadUIIndex( team )




		return Squads_GetSquadColor( squadIndex )

}



string function TreasureHunt_GetTeamName( int team )
{
	int squadIndex = Squads_GetSquadUIIndex( team )

	return Squads_GetSquadName( squadIndex )
}



asset function TreasureHunt_GetTeamIcon( int team )
{
	int squadIndex = Squads_GetSquadUIIndex( team )

	return Squads_GetSquadIcon( squadIndex )
}



struct TeamScoreStruct
{
	int teamNum = 0
	int score = 0
	bool isVisible = false
}

void function TreasureHunt_DisplaySquadScore_Thread()
{
	var rui = CreateCockpitPostFXRui( $"ui/lockdown_score.rpak", MINIMAP_Z_BASE + 10 )
	file.lockdownScoreHud = rui

	OnThreadEnd(
		function() : ( rui )
		{
			RuiDestroyIfAlive( rui )
		}
	)


		bool prevShouldTriggerCrowdOneShot = false


	int numTeams = GetNumTeamsExisting()
	Assert( numTeams <= MAX_SUPPORTED_TEAMS, "LOCKDOWN: Trying to update scores for " + numTeams + " but this score RUI only supports " + MAX_SUPPORTED_TEAMS + " teams" )
	int maxTeamSize = GetMaxTeamSizeForPlaylist( GetCurrentPlaylistName() )
	int scoreLimit = GetScoreLimit_FromPlaylist()
	RuiSetInt( file.lockdownScoreHud, "scoreLimit", scoreLimit )

	while( GetGameState() < eGameState.WinnerDetermined )
	{
		array < entity > allPlayersArray = GetPlayerArray()
		table < int, array< int > > zoneToTeamsOnZoneTable
		int objectiveLocalPlayerIsOn = -1
		foreach ( entity player in allPlayersArray )
		{
			if ( !IsValid( player ) )
				continue

			Squads_SetCustomPlayerInfo( player )

			
			int objectiveOccupiedByPlayer = player.GetPlayerNetInt( "treasureHunt_OccupiedObjectiveID" )
			int playerTeam = player.GetTeam()
			if ( TreasureHunt_IsValidObjectiveIndex( objectiveOccupiedByPlayer ) )
			{
				if ( !( objectiveOccupiedByPlayer in zoneToTeamsOnZoneTable) ) 
					zoneToTeamsOnZoneTable[ objectiveOccupiedByPlayer ] <- [ playerTeam ]
				else if ( !zoneToTeamsOnZoneTable[ objectiveOccupiedByPlayer ].contains( playerTeam ) ) 
					zoneToTeamsOnZoneTable[ objectiveOccupiedByPlayer ].append( playerTeam )

				
				if ( player == GetLocalClientPlayer() )
				{
					objectiveLocalPlayerIsOn = objectiveOccupiedByPlayer
				}
			}
		}

		
		for ( int index = 0; index < file.objectiveWaypoints.len(); index++ )
		{
			if ( !( index in zoneToTeamsOnZoneTable ) )
			{
				
				if ( file.objectiveWaypoints[ index ] != null )
					zoneToTeamsOnZoneTable[ index ] <- []
				else 
					TreasureHunt_ScoreHud_SetVisibleCapturePointName( index, "" )
			}
		}

		
		foreach ( objectiveID, teamsOnObjectiveArray in zoneToTeamsOnZoneTable )
		{
			string objectiveName = CaptureObjectivePing_GetObjectiveNameFromObjectiveID_Localized( objectiveID )

			
			TreasureHunt_ScoreHud_SetVisibleCapturePointName( objectiveID, objectiveName )

			if ( teamsOnObjectiveArray.len() == 1 ) 
			{
				int team = teamsOnObjectiveArray[ 0 ]
				int squadIndex = Squads_GetSquadUIIndex( team )
				TreasureHunt_ScoreHud_SetCapturePointColor( objectiveID, TreasureHunt_GetTeamColor( team ) )
				TreasureHunt_ScoreHud_SetCapturePointContested( objectiveID, false )
				TreasureHunt_ScoreHud_SetTeamOnPoint( objectiveID, squadIndex, true )

				
				if ( objectiveID == objectiveLocalPlayerIsOn )
				{
					TreasureHunt_CaptureZoneHud_SetStatusColor( TreasureHunt_GetTeamColor( team ) )
					TreasureHunt_CaptureZoneHud_SetStatusText( "#FREEDM_LOCKDOWN_OBJ_STATE_CAPTURING" )
					TreasureHunt_CaptureZoneHud_SetStatusIcon( TreasureHunt_GetTeamIcon( team ) )
					TreasureHunt_CaptureZoneHud_SetObjectiveName( objectiveName )
					TreasureHunt_CaptureZoneHud_SetContested( false )

					float objStartTime = Time()
					if ( IsValid( file.objectiveWaypoints[ objectiveID ] ) )
						objStartTime = file.objectiveWaypoints[ objectiveID ].GetWaypointFloat( CAPTURE_OBJ_WAYPOINT_FLOAT_IDX_START_TIME )

					float objEndTime = objStartTime + GetTreasureHuntZoneHoldTime()

					TreasureHunt_CaptureZoneHud_SetZoneStartTime( objStartTime )
					TreasureHunt_CaptureZoneHud_SetZoneEndTime( objEndTime )
					TreasureHunt_CaptureZoneHud_SetZoneDuration( GetTreasureHuntZoneHoldTime() )

					int objectiveState = eTreasureHuntCaptureZoneState.CAPTURING
					if ( IsValid( file.objectiveWaypoints[ objectiveID ] ) )
						objectiveState = file.objectiveWaypoints[ objectiveID ].GetWaypointInt( CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_STATE )
					TreasureHunt_CaptureZoneHud_SetZoneState( objectiveState )

					TreasureHunt_CaptureZoneStatus_SetRuiVisibility( true )
				}
			}
			else if ( teamsOnObjectiveArray.len() > 1 ) 
			{
				int objectiveState = eTreasureHuntCaptureZoneState.CONTESTED
				if ( IsValid( file.objectiveWaypoints[ objectiveID ] ) )
					objectiveState = file.objectiveWaypoints[ objectiveID ].GetWaypointInt( CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_STATE )

				vector color = objectiveState == eTreasureHuntCaptureZoneState.SUDDEN_DEATH ? SrgbToLinear( ZONE_SUDDENDEATH_COLOR / 255 ) : SrgbToLinear( ZONE_CONTESTED_COLOR /255 )
				TreasureHunt_ScoreHud_SetCapturePointColor( objectiveID, color )
				TreasureHunt_ScoreHud_SetCapturePointContested( objectiveID, true )

				
				if ( objectiveID == objectiveLocalPlayerIsOn )
				{
					string statusText = objectiveState == eTreasureHuntCaptureZoneState.SUDDEN_DEATH ? "#FREEDM_LOCKDOWN_OBJ_STATE_SUDDENDEATH" : "#FREEDM_LOCKDOWN_OBJ_STATE_CONTESTED"
					TreasureHunt_CaptureZoneHud_SetStatusColor( color )
					TreasureHunt_CaptureZoneHud_SetStatusText( statusText )
					TreasureHunt_CaptureZoneHud_SetStatusIcon( $"rui/gamemodes/tdm/tdm_contested_icon" )
					TreasureHunt_CaptureZoneHud_SetObjectiveName( objectiveName )
					TreasureHunt_CaptureZoneHud_SetContested( true )

					float objStartTime = Time()
					if ( IsValid( file.objectiveWaypoints[ objectiveID ] ) )
						objStartTime = file.objectiveWaypoints[ objectiveID ].GetWaypointFloat( CAPTURE_OBJ_WAYPOINT_FLOAT_IDX_START_TIME )

					float objEndTime = objStartTime + GetTreasureHuntZoneHoldTime()

					TreasureHunt_CaptureZoneHud_SetZoneStartTime( objStartTime )
					TreasureHunt_CaptureZoneHud_SetZoneEndTime( objEndTime )
					TreasureHunt_CaptureZoneHud_SetZoneDuration( GetTreasureHuntZoneHoldTime() )
					TreasureHunt_CaptureZoneHud_SetZoneState( objectiveState )

					TreasureHunt_CaptureZoneStatus_SetRuiVisibility( true )
				}
			}
			else 
			{
				TreasureHunt_ScoreHud_SetCapturePointColor( objectiveID, ZONE_NEUTRAL_COLOR )
				TreasureHunt_ScoreHud_SetCapturePointContested( objectiveID, false )
			}

			
			for ( int team = TEAM_IMC; team < TEAM_IMC + MAX_SUPPORTED_TEAMS; team++ )
			{
				int squadIndex = Squads_GetSquadUIIndex( team )

				if ( teamsOnObjectiveArray.contains( team ) )
					TreasureHunt_ScoreHud_SetTeamOnPoint( objectiveID, squadIndex, true )
				else
					TreasureHunt_ScoreHud_SetTeamOnPoint( objectiveID, squadIndex, false )
			}
		}

		
		if ( objectiveLocalPlayerIsOn == -1 )
		{
			TreasureHunt_CaptureZoneStatus_SetRuiVisibility( false )
		}

		
		entity localPlayer = GetLocalClientPlayer()
		array< TeamScoreStruct > teamScores = []
		for ( int team = TEAM_IMC; team < TEAM_IMC + MAX_SUPPORTED_TEAMS; team++ )
		{
			int squadIndex = Squads_GetSquadUIIndex( team )

			array< entity > playersOnTeam = GetPlayerArrayOfTeam( team )
			bool isTeamVisible = true
			if ( playersOnTeam.len() <= 0 )
			{
				isTeamVisible = false
				RuiSetBool( file.lockdownScoreHud, "squadVisible" + squadIndex, false )
			}

			RuiSetBool( file.lockdownScoreHud, "yourTeam" + squadIndex, IsPlayerOnTeam( localPlayer, team ) )

			int teamScore = GameRules_GetTeamScore( team )
			RuiSetInt( file.lockdownScoreHud, "score_" + squadIndex, teamScore )

			TeamScoreStruct teamScoreData
			teamScoreData.teamNum = squadIndex
			teamScoreData.score = teamScore
			teamScoreData.isVisible = isTeamVisible
			teamScores.push( teamScoreData )

			if ( isTeamVisible )
				TreasureHunt_SetCharacterInfo( file.lockdownScoreHud, team, squadIndex )
		}

		
		teamScores.sort( TreasureHunt_OrderTeamsByScore )

		
		for ( int placement = 0; placement < MAX_SUPPORTED_TEAMS; placement++ )
		{
			int teamNum = teamScores[ placement ].teamNum
			RuiSetInt( file.lockdownScoreHud, "placementTeam" + teamNum, placement )
		}


			entity localViewPlayer = GetLocalViewPlayer()
			bool shouldTriggerCrowdOneShot = false
			if ( IsValid( localViewPlayer ) )
			{
				foreach ( int objectiveID, array< int > teamsOnObjectiveArray in zoneToTeamsOnZoneTable )
				{
					foreach ( int teamOnObjective in teamsOnObjectiveArray )
					{
						shouldTriggerCrowdOneShot = IsPlayerOnTeam( localViewPlayer, teamOnObjective )

						if ( shouldTriggerCrowdOneShot )
							break
					}

					if ( shouldTriggerCrowdOneShot )
						break
				}
			}

			if ( shouldTriggerCrowdOneShot != prevShouldTriggerCrowdOneShot )
			{
				
				ToggleCrowdSoundOnEntity( localViewPlayer, eCrowdSound.CAPTURE_START, shouldTriggerCrowdOneShot )
				prevShouldTriggerCrowdOneShot = shouldTriggerCrowdOneShot
			}


		WaitFrame()
	}
}



void function TreasureHunt_SetCharacterInfo( var rui, int team, int reorderedIndex )
{
	int squadIndex = Squads_GetSquadUIIndex( team )
	string indexString = string( squadIndex )

	RuiSetAsset( rui, "squadImage" + indexString, Squads_GetSquadIcon(reorderedIndex ) )
	RuiSetString( rui, "squadName" + indexString, Squads_GetSquadName(reorderedIndex ) )
	RuiSetBool( rui, "squadVisible" + indexString, true )
	RuiSetColorAlpha( rui, "squadBorderColor" + indexString, Squads_GetSquadColor( reorderedIndex ), 1.0 )
}



int function TreasureHunt_OrderTeamsByScore( TeamScoreStruct scoreA, TeamScoreStruct scoreB )
{
	
	if ( scoreA.isVisible && !scoreB.isVisible )
		return -1
	else if ( !scoreA.isVisible && scoreB.isVisible )
		return 1

	
	if ( scoreA.score > scoreB.score )
		return -1
	else if ( scoreA.score < scoreB.score )
		return 1
	return 0
}



void function TreasureHunt_CreateCaptureZoneStatusRui()
{
	if ( file.captureZoneStatusRui != null && RuiIsAlive( file.captureZoneStatusRui ) )
		return

	file.captureZoneStatusRui = CreateCockpitPostFXRui( $"ui/lockdown_capture_zone_status.rpak", MINIMAP_Z_BASE + 10 )
}



void function TreasureHunt_CaptureZoneStatus_SetRuiVisibility( bool isVisible )
{
	if ( file.captureZoneStatusRui == null || !RuiIsAlive( file.captureZoneStatusRui ) )
		return

	RuiSetBool( file.captureZoneStatusRui, "isVisible", isVisible )
}



void function TreasureHunt_ScoreHud_SetVisibleCapturePointName( int pointIndex, string pointName )
{
	if ( file.lockdownScoreHud == null && file.lockdownScoreboardHud == null )
		return

	if ( file.lockdownScoreHud != null && RuiIsAlive( file.lockdownScoreHud ) )
	{
		RuiSetString( file.lockdownScoreHud, "availablePoint" + pointIndex, pointName )
	}

	if ( file.lockdownScoreboardHud != null && RuiIsAlive( file.lockdownScoreboardHud ) )
	{
		RuiSetString( file.lockdownScoreboardHud, "availablePoint" + pointIndex , pointName )
	}
}



void function TreasureHunt_ScoreHud_SetCapturePointContested( int pointIndex, bool isContested )
{
	if ( file.lockdownScoreHud == null && file.lockdownScoreboardHud == null )
		return

	if ( file.lockdownScoreHud != null && RuiIsAlive( file.lockdownScoreHud )  )
	{
		RuiSetBool( file.lockdownScoreHud, "point" + pointIndex + "Contested", isContested )
	}

	if ( file.lockdownScoreboardHud != null && RuiIsAlive( file.lockdownScoreboardHud ) )
	{
		RuiSetBool( file.lockdownScoreboardHud, "point" + pointIndex + "Contested", isContested )
	}
}



void function TreasureHunt_ScoreHud_SetCapturePointColor( int pointIndex, vector pointColor )
{
	if ( file.lockdownScoreHud == null && file.lockdownScoreboardHud == null )
		return

	if ( file.lockdownScoreHud != null && RuiIsAlive( file.lockdownScoreHud ) )
	{
		RuiSetColorAlpha( file.lockdownScoreHud, "pointColor" + pointIndex, pointColor, 1 )
	}

	if ( file.lockdownScoreboardHud != null && RuiIsAlive( file.lockdownScoreboardHud ) )
	{
		RuiSetColorAlpha( file.lockdownScoreboardHud, "pointColor" + pointIndex, pointColor, 1 )
	}
}



void function TreasureHunt_ScoreHud_SetTeamOnPoint( int pointIndex, int teamId, bool isOnPoint )
{
	if ( file.lockdownScoreHud == null && file.lockdownScoreboardHud == null )
		return

	if ( file.lockdownScoreHud != null && RuiIsAlive( file.lockdownScoreHud ) )
	{
		RuiSetBool( file.lockdownScoreHud, "team" + teamId + "OnPoint" + pointIndex, isOnPoint )
	}

	if ( file.lockdownScoreboardHud != null && RuiIsAlive( file.lockdownScoreboardHud ) )
	{
		RuiSetBool( file.lockdownScoreboardHud, "team" + teamId + "OnPoint" + pointIndex, isOnPoint )
	}
}



void function TreasureHunt_CaptureZoneHud_SetStatusColor( vector statusColor )
{
	if ( file.captureZoneStatusRui != null && RuiIsAlive( file.captureZoneStatusRui ) )
	{
		RuiSetColorAlpha( file.captureZoneStatusRui, "statusColor", statusColor, 1.0 )
	}
}



void function TreasureHunt_CaptureZoneHud_SetStatusText( string statusText )
{
	if ( file.captureZoneStatusRui != null && RuiIsAlive( file.captureZoneStatusRui ) )
	{
		RuiSetString( file.captureZoneStatusRui, "statusText", Localize( statusText ) )
	}
}



void function TreasureHunt_CaptureZoneHud_SetStatusIcon( asset statusIcon )
{
	if ( file.captureZoneStatusRui != null && RuiIsAlive( file.captureZoneStatusRui ) )
	{
		RuiSetAsset( file.captureZoneStatusRui, "statusIcon", statusIcon )
	}
}



void function TreasureHunt_CaptureZoneHud_SetZoneStartTime( float captureStartTime )
{
	if ( file.captureZoneStatusRui != null && RuiIsAlive( file.captureZoneStatusRui ) )
	{
		RuiSetFloat( file.captureZoneStatusRui, "zoneStartTime", captureStartTime )
	}
}



void function TreasureHunt_CaptureZoneHud_SetZoneEndTime( float captureEndTime )
{
	if ( file.captureZoneStatusRui != null && RuiIsAlive( file.captureZoneStatusRui ) )
	{
		RuiSetFloat( file.captureZoneStatusRui, "zoneEndTime", captureEndTime )
	}
}



void function TreasureHunt_CaptureZoneHud_SetZoneDuration( float captureDuration )
{
	if ( file.captureZoneStatusRui != null && RuiIsAlive( file.captureZoneStatusRui ) )
	{
		RuiSetFloat( file.captureZoneStatusRui, "zoneDuration", captureDuration )
	}
}



void function TreasureHunt_CaptureZoneHud_SetZoneState( int zoneState )
{
	if ( file.captureZoneStatusRui != null && RuiIsAlive( file.captureZoneStatusRui ) )
	{
		RuiSetInt( file.captureZoneStatusRui, "objectiveState", zoneState )
	}
}



void function TreasureHunt_CaptureZoneHud_SetObjectiveName( string objectiveName )
{
	if ( file.captureZoneStatusRui != null && RuiIsAlive( file.captureZoneStatusRui ) )
	{
		RuiSetString( file.captureZoneStatusRui, "objectiveName", Localize( objectiveName ) )
	}
}



void function TreasureHunt_CaptureZoneHud_SetContested( bool isContested )
{
	if ( file.captureZoneStatusRui != null && RuiIsAlive( file.captureZoneStatusRui ) )
	{
		RuiSetBool( file.captureZoneStatusRui, "isContested", isContested )
	}
}











































































































const float SPLASH_TEXT_DURATION = 4.0
void function ServerCallback_TreasureHunt_DisplayMessageToClient( int messageID, int pointsValue )
{
	entity localPlayer = GetLocalViewPlayer()

	if (  !IsValid( localPlayer )  )
		return

	string messageText = ""

	string announcementText = ""
	string announcementSubText = ""
	bool isObituaryMessage = false
	bool isAnnouncementMessage = false
	bool isSplashText = false
	vector announcementColor = <0.8, 0.8, 0.8>

	switch ( messageID )
	{
		case eTreasureHuntMessageIndex.SCORE_CAPTURE_INITIATED:
			isObituaryMessage = true
			messageText = Localize( "#FREEDM_LOCKDOWN_CAPTURE_STARTED", pointsValue )
			break
		case eTreasureHuntMessageIndex.SCORE_CAPTURE_INITIATED_PERSONAL:
			isObituaryMessage = true
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_CAPTURE_STARTED_YOU", pointsValue )
			EmitUISound( LOCKDOWN_SFX_OBJ_CAPTURE_STARTED )
			break
		case eTreasureHuntMessageIndex.SCORE_CAPTURING:
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_CAPTURING", pointsValue )
			EmitUISound( LOCKDOWN_SFX_OBJ_CAPTURING )
			break
		case eTreasureHuntMessageIndex.SCORE_OBJECTIVE_CAPTURED:
			isObituaryMessage = true
			messageText = Localize( "#FREEDM_LOCKDOWN_CAPTURED", pointsValue )
			break
		case eTreasureHuntMessageIndex.SCORE_OBJECTIVE_CAPTURED_PERSONAL:
			isObituaryMessage = true
			isAnnouncementMessage = true
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_CAPTURED_YOU", pointsValue )
			announcementText = Localize( "#FREEDM_LOCKDOWN_CAPTURED_YOU_ANNOUNCE" )
			announcementSubText = Localize( "#FREEDM_LOCKDOWN_POINTS", pointsValue )
			EmitUISound( LOCKDOWN_SFX_OBJ_CAPTURE_FINISHED )
			break
		case eTreasureHuntMessageIndex.SCORE_OBJECTIVE_CAPTURED_SUDDENDEATH:
			isObituaryMessage = true
			messageText = Localize( "#FREEDM_LOCKDOWN_CAPTURED_SUDDENDEATH", pointsValue )
			break
		case eTreasureHuntMessageIndex.SCORE_OBJECTIVE_CAPTURED_SUDDENDEATH_PERSONAL:
			isObituaryMessage = true
			isAnnouncementMessage = true
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_CAPTURED_SUDDENDEATH_YOU", pointsValue )
			announcementText = Localize( "#FREEDM_LOCKDOWN_CAPTURED_SUDDENDEATH_YOU_ANNOUNCE" )
			announcementSubText = Localize( "#FREEDM_LOCKDOWN_POINTS", pointsValue )
			EmitUISound( LOCKDOWN_SFX_OBJ_CAPTURE_FINISHED )
			break
		case eTreasureHuntMessageIndex.SCORE_KILL:
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_KILL_SCORE", pointsValue )
			break
		case eTreasureHuntMessageIndex.SCORE_KILL_FROM_ZONE:
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_KILL_FROM_ZONE_SCORE", pointsValue )
			break
		case eTreasureHuntMessageIndex.SCORE_KILL_WINNER:
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_KILL_WINNER_SCORE", pointsValue )
			break
		case eTreasureHuntMessageIndex.CAPTURE_OBJECTIVE_INCOMING:
			isAnnouncementMessage = true
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_CAPTURE_OBJ_INCOMING" )
			EmitUISound( LOCKDOWN_SFX_CAPTURE_OBJ_INCOMING )
			break
		case eTreasureHuntMessageIndex.OBJECTIVE_ENTERS_SUDDEN_DEATH:
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_OBJ_SUDDEN_DEATH" )
			EmitUISound( LOCKDOWN_SFX_CAPTURE_OBJ_SUDDEN_DEATH )
			break
		case eTreasureHuntMessageIndex.TEAM_HALFWAY_TO_WIN:
			isAnnouncementMessage = true
			isSplashText = true
			int squadIndex = Squads_GetSquadUIIndex( GamemodeUtility_GetWinningTeamOrAlliance( false ) )
			messageText = Localize( "#FREEDM_LOCKDOWN_TEAM_HALFWAY_TO_WIN", Localize( Squads_GetSquadNameLong( squadIndex, false ) ) )
			EmitUISound( LOCKDOWN_SFX_TEAM_HALFWAY_TO_WIN )
			break
		case eTreasureHuntMessageIndex.TEAM_HALFWAY_TO_WIN_PERSONAL:
			isAnnouncementMessage = true
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_TEAM_HALFWAY_TO_WIN", Localize( "#FREEDM_LOCKDOWN_YOUR_TEAM_WINNING_PREFIX" ) )
			EmitUISound( LOCKDOWN_SFX_TEAM_HALFWAY_TO_WIN )
			break
		case eTreasureHuntMessageIndex.TEAM_CLOSE_TO_WIN:
			isAnnouncementMessage = true
			isSplashText = true
			int squadIndex = Squads_GetSquadUIIndex( GamemodeUtility_GetWinningTeamOrAlliance( false ) )
			messageText = Localize( "#FREEDM_LOCKDOWN_TEAM_CLOSE_TO_WIN", Localize( Squads_GetSquadNameLong( squadIndex, false ) ) )
			EmitUISound( LOCKDOWN_SFX_TEAM_CLOSE_TO_WIN )
			break
		case eTreasureHuntMessageIndex.TEAM_CLOSE_TO_WIN_PERSONAL:
			isAnnouncementMessage = true
			isSplashText = true
			messageText = Localize( "#FREEDM_LOCKDOWN_TEAM_CLOSE_TO_WIN", Localize( "#FREEDM_LOCKDOWN_YOUR_TEAM_WINNING_PREFIX" ) )
			EmitUISound( LOCKDOWN_SFX_TEAM_CLOSE_TO_WIN )
			break
		case eTreasureHuntMessageIndex.OBJECTIVE_ENTERS_CONTESTED_STATE:
			EmitUISound( LOCKDOWN_SFX_OBJECTIVE_CONTESTED )
			break
		case eTreasureHuntMessageIndex.TEAM_GAINED_LEAD:
			EmitUISound( LOCKDOWN_SFX_TEAM_GAINED_LEAD )
			break
		case eTreasureHuntMessageIndex.TEAM_LOST_LEAD:
			EmitUISound( LOCKDOWN_SFX_TEAM_LOST_LEAD )
			break
		default:
			Assert( false, "LOCKDOWN: " + FUNC_NAME() + " was run with an Invalid messageID: " + messageID + " , message will not display in Live for this event." )
			return
	}

	
	if ( announcementText == "" )
		announcementText = messageText

	if ( isObituaryMessage )
		Obituary_Print_Localized( messageText )

	if ( isAnnouncementMessage )
	{
		entity localViewPlayer = GetLocalViewPlayer()

		AnnouncementData announcement = Announcement_Create( announcementText )
		announcement.subText = announcementSubText
		announcement.duration = 5.0
		announcement.titleColor = announcementColor
		announcement.priority =	0
		announcement.announcementStyle = ANNOUNCEMENT_STYLE_CIRCLE_WARNING

		AnnouncementFromClass( localPlayer, announcement )
	}

	if ( isSplashText )
		AddImageQueueMessage( messageText, $"", SPLASH_TEXT_DURATION )
}















void function TreasureHunt_PlayCaptureZoneEnterExitSFX( bool isEnteringZone )
{
	entity localPlayer = GetLocalViewPlayer()

	if ( !IsValid( localPlayer ) )
		return

	if ( isEnteringZone )
	{
		EmitUISound( LOCKDOWN_SFX_CAPTURE_ZONE_ENTER )
	}
	else
	{
		EmitUISound( LOCKDOWN_SFX_CAPTURE_ZONE_EXIT )
	}
}


















































bool function TreasureHunt_IsTreasureHuntObjectiveCommsAction( int commsAction, entity subjectEnt )
{
	bool isTreasureHuntObjectiveCommsAction = false

	if ( GameModeVariant_IsActive( eGameModeVariants.FREEDM_LOCKDOWN ) )
	{
		if ( commsAction == eCommsAction.PING_TREASUREHUNT_OBJ_ATTACK )
		{
			entity owner = subjectEnt.GetOwner()
			if ( IsValid( owner ) && IsPlayerWaypoint( owner ) && owner.GetWaypointType() == eWaypoint.TREASUREHUNT_OBJECTIVE )
				isTreasureHuntObjectiveCommsAction = true
		}
	}

	return isTreasureHuntObjectiveCommsAction
}




int function TreasureHunt_GetObjectiveIDFromWaypoint( entity waypoint )
{
	return waypoint.GetWaypointInt( CAPTURE_OBJ_WAYPOINT_INT_IDX_OBJ_INDEX )
}






entity function TreasureHunt_GetStarterPingFromTraceBlockerPing( entity pingedEnt, int playerTeam )
{
	entity starterPing = null

	if ( IsValid( pingedEnt ) && pingedEnt.GetScriptName() == TREASUREHUNT_OBJECTIVE_SCRIPTNAME && IsValid( pingedEnt.GetOwner() ) )
	{
		array < entity > objectiveStarterPings = CaptureObjectivePing_GetStarterPingsArray()
		if ( objectiveStarterPings.len() > 0 )
		{
			entity objective = pingedEnt.GetOwner()

			foreach ( ping in objectiveStarterPings )
			{
				if ( !IsValid( ping ) )
					continue

				
				if ( IsValid( ping ) && IsValid( ping.GetParent() ) && IsValid( ping.GetParent().GetOwner() ) )
				{
					entity pingedObjective = ping.GetParent().GetOwner()
					if ( pingedObjective == objective && playerTeam == ping.GetTeam() )
						starterPing = ping
				}
			}
		}
	}

	return starterPing
}




































































































































      