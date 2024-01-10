















global function ShadowArmy_Init
global function ShGameMode_ShadowArmy_RegisterNetworking
global function IsShadowArmyGamemode
global function IsShadowArmyGamemodeCineVersion
global function ShadowArmy_GetNumRevSquadsForMatchStart















global function ShadowArmy_ServerCallback_ShowAnnouncementMessage
global function ShadowArmy_ServerCallback_ShowHinttMessage
global function ShadowArmy_ServerCallback_GivePlayerRepeatingEnemyMapScans
global function ShadowArmy_ServerCallback_UpdateEvacTargetCountOnHud
global function ShadowArmy_ServerCallback_SetDeathScreenCallbacks
global function ShadowArmy_ServerCallback_PlayMatchEndMusic
global function ShadowArmy_ServerCallback_SetAllianceAssignmentCompleteFlag
global function ShadowArmy_ServerCallback_DestroyLegendStartAreaMapFeature
global function ShadowArmy_PlayIntroBannerSoundForLegends_Thread



global function ShadowArmy_ShouldEnterBleedout
global function ShadowArmy_IsPlayerOnShadowArmy
global function ShadowArmy_IsPlayerInShadowForm
global function ShadowArmy_GetEnemySquadPlayersForAllianceIntro
global function ShadowArmy_GetLegendSpawnGroupsNumber














global const float SHADOWARMY_LEGEND_SPAWN_FADE_FROM_BLACK_HOLD_DURATION = 1.0
global const float SHADOWARMY_LEGEND_SPAWN_FADE_FROM_BLACK_FADE_DURATION = 2.0

const int SHADOWARMY_VICTORY_FLAGS_UNKNOWN = 0
const int SHADOWARMY_VICTORY_FLAGS_ELIMINATION = ( 1 << 1 )
global const int SHADOWARMY_VICTORY_FLAGS_EVAC = ( 1 << 2 )
const int SHADOWARMY_VICTORY_FLAGS_PREVENTED_EVAC = ( 1 << 3 )
const int SHADOWARMY_VICTORY_FLAGS_FORFEIT = ( 1 << 4 )

const float SHADOWARMY_DEFAULT_SHORT_SPAWN_DELAY = 5.0
const float SHADOWARMY_SHORTEST_POSSIBLE_SPAWN_DELAY = 1.0
const array < int > SHADOW_ARMY_ALLOWED_ALLIANCE_PINGS = [ ePingType.BLEEDOUT ]


global const string SHADOWARMY_MAP_EVAC_AREA = "shadowarmy_evac_area"
global const string SHADOWARMY_LEGEND_START_AREA = "shadowarmy_legend_start_area"
const string SHADOWARMY_PIN_VICTORYCONDITION_UNKNOWN = "unknown"
const string SHADOWARMY_PIN_VICTORYCONDITION_ELIMINATION = "elimination"
const string SHADOWARMY_PIN_VICTORYCONDITION_EVAC = "evac"
const string SHADOWARMY_PIN_VICTORYCONDITION_PREVENTED_EVAC = "prevented_evac"
const string SHADOWARMY_PIN_VICTORYCONDITION_FORFEIT = "team_forfeit"

const asset DEATH_SCREEN_RUI = $"ui/shadowarmy_squad_summary_header_data.rpak"

global const int SHADOWARMY_LEGEND_ALLIANCE = ALLIANCE_A
global const int SHADOWARMY_REVENANT_ALLIANCE = ALLIANCE_B
const int MAX_EVAC_TARGET_COUNT_FOR_LEGENDS = 6 


const float SHADOWARMY_ANNOUNCEMENT_DURATION = 5.0
const asset ANNOUNCEMENT_LOBA_ICON_FG = $"rui/gamemodes/rev_army/loba_banner_icon_outline"
const asset ANNOUNCEMENT_SHADOW_REV_ICON_FG = $"rui/gamemodes/rev_army/soldier_rev_banner_icon_outline"
const asset ANNOUNCEMENT_FULL_REV_ICON_FG = $"rui/gamemodes/rev_army/red_eye_rev_banner_icon_outline"
const vector ANNOUNCEMENT_FULL_REV_BANNER_COLOR = < 1, 0, 0 >
const vector EVAC_INCOMING_HUD_ELEMENT_COL = < 0.945, 0.725, 0.1 >
const vector EVAC_ARRIVED_HUD_ELEMENT_COL = < 0.956, 0.192, 0.192 >


global const string SFX_SHADOWARMY_REV_ALLIANCE_CHAR_SEL_START = "Lobby_RevenantArmy_Menu_Team_Attributed"
const string SFX_SHADOWARMY_LEGEND_SPAWN = "InGame_RevenantArmy_FadeFromBlack_Intro"
const string SFX_SHADOWARMY_REVENGE_KILL_MSG = "UI_InGame_ShadowSquad_RevengeKill"
const string SFX_SHADOWARMY_END_MSG = "UI_InGame_ShadowSquad_FinalSquadMessage"
const string SFX_SHADOWARMY_EVAC_MSG_STINGER_NORMAL = "UI_RevenantArmy_EvacSoon_Stinger"
const string SFX_SHADOWARMY_REV_ARMY_REV_RESPAWN_STINGER = "UI_RevenantArmy_Respawning_NormalRev"
const string SFX_SHADOWARMY_REDEYE_REV_RESPAWN_STINGER = "UI_RevenantArmy_Respawning_RedEyedRev"
const string SFX_SHADOWARMY_MATCH_WIN_STINGER = "UI_RevenantArmy_MatchWon_Stinger"
const string SFX_SHADOWARMY_MATCH_LOSE_STINGER = "UI_RevenantArmy_MatchLost_Stinger"


const string MUSIC_SHADOWARMY_LEGEND_VICTORY = "Music_RevArmy_Victory_Legends"
const string MUSIC_SHADOWARMY_REV_VICTORY = "Music_RevArmy_Victory_Revenants"
const string MUSIC_SHADOWARMY_LEGEND_SPAWN_AS_REV = "Music_RevArmy_Spawn_RevenantArmy"
const string MUSIC_SHADOWARMY_GAMEPLAY_RAMPUP_LEGEND = "Music_RevArmy_Gameplay_Legends"
const string MUSIC_SHADOWARMY_GAMEPLAY_RAMPUP_REV = "Music_RevArmy_Gameplay_Revenants"
const array< float > MUSIC_RAMPUP_CONTROLLER_VALUES = [ 50.0, 150.0, 200.0 ] 
const int MUSIC_RAMPUP_LEVEL_NOT_SET = -1


const asset EVAC_AREA_ICON = $"rui/gamemodes/shadow_squad/evac_countdown"
const asset LEGEND_START_AREA_ICON = $"rui/hud/ping/icon_ping_attack"






























































const array<string> SHADOWARMY_DISABLED_BATTLE_CHATTER_EVENTS = [
	"bc_anotherSquadAttackingUs",
	"bc_championEliminated",
	"bc_congratsKill",
	"bc_iDownedAnEnemy",
	"bc_iDownedAnotherEnemy",
	"bc_iKilledAnEnemy",
	"bc_returnFromRespawn",
	"bc_squadTeamWipe",
	"bc_tactical",
	"bc_takingFire",
	"bc_imJumpmaster",
	"bc_shieldBreakEnemy",
]


const bool SHADOW_ARMY_SHOW_DETAILED_DEBUG = true
#if DEV
const bool SHADOWARMY_DISPLAY_PLAYERSPAWN_DEBUG_DRAWS = false
const float SHADOWARMY_DEBUG_DRAW_DISPLAY_TIME = 20.0
const float SHADOWARMY_SPAWN_DEBUG_DRAW_RADIUS = 150.0
#endif

enum eShadowArmyGamePhase
{
	GAME_START,
	WAITING_FOR_EVAC_OBJECTIVE,
	EVAC_OBJECTIVE,
	EVAC_SHIP_ARRIVED,
	EVAC_SHIP_DEPARTED,

	_count
}

global enum eShadowArmyMessageIndex
{
	LTM_DROP_ANNOUNCE_LEGEND,
	LTM_DROP_ANNOUNCE_SHADOW,
	RESPAWNING_AS_SHADOW_FIRST_TIME,
	RESPAWNING_AS_SHADOW,
	RESPAWNING_AS_SHADOW_FROM_LEGEND,
	RESPAWNING_AS_FULL_REV,
	RESPAWNING_AS_LEGEND,
	RESPAWNING_AS_LEGEND_EVAC,
	FULL_REV_SPAWNED_LEGEND,
	FULL_REV_SPAWNED_SHADOW,
	FULL_REV_KILLED,
	LEGEND_TEAM_SWITCHED_TO_REV,
	EVAC_CALLED_IN_RING_LEGEND,
	EVAC_CALLED_IN_RING_SHADOW,
	EVAC_CALLED_IN_EMERGENCY_LEGEND,
	EVAC_CALLED_IN_EMERGENCY_SHADOW,
	EVAC_ARRIVED_LEGEND,
	EVAC_ARRIVED_SHADOW,
	SAFE_ON_EVAC_SHIP,
	ALLIANCE_SWITCH_DISABLED_LEGEND,
	ALLIANCE_SWITCH_DISABLED_SHADOW,
	LEGENDS_RESPAWNED,
	LEGEND_ENTERED_EVAC_SHIP,
	EVAC_AREA_UPDATED,
	EVAC_LOCATION_REVEALED,
	BLANK,
	_count
}

global enum eShadowArmyMessageType
{
	ANNOUNCE_ONLY,
	OBIT_ONLY,
	ANNOUNCE_AND_OBIT,
	_count
}

enum eShadowArmyHintIndex
{
	FULL_REV_CANDIDATE_HINT,
	FULL_REV_CRITERIA_NO_DAM_HINT,
	_count
}

enum eShadowArmyRespawnForm
{
	LIVING_LEGEND,
	FULL_REVENANT,
	SHADOW,
	_count
}


enum eShadowArmyMusicRampLevels
{
	PRE_EVAC_SEQUENCE,
	EVAC_SEQUENCE_STARTED,
	EVAC_ARRIVED,
	_count
}













struct
{































	int evacTargetCount = 0



	bool areEnemyMapScansActiveOnClient = false

	
	entity musicEntity 
	int musicRampUpLevel = MUSIC_RAMPUP_LEVEL_NOT_SET
	array < entity > playersRespawningAfterAllianceSwitch

} file

void function ShadowArmy_Init()
{
	if ( !IsShadowArmyGamemode() )
		return




















































































		SURVIVAL_SetGameStateAssetOverrideCallback( OverrideGameState )
		AddCallback_OnPlayerLifeStateChanged( OnPlayerLifeStateChanged_Client )
		AddCallback_GameStateEnter( eGameState.Playing, ShadowArmy_OnGamestateEnterPlaying_Client )
		AddCallback_AllianceProximity_OnTeamPutIntoAlliance( ShadowArmy_OnTeamChangedAlliance_Client )
		AddCallback_ClientOnPlayerConnectionStateChanged( ShadowArmy_OnPlayerConnectionStateChanged )
		AddCallback_OnYouRespawned( OnYouRespawned )
		AddOnSpectatorTargetChangedCallback( ShadowArmy_OnSpectateTargetChanged )
		Survival_SetVictorySoundPackageFunction( GetVictorySoundPackage )
		AddCallback_OnSurvivalDeathFieldStageChanged( ShadowArmy_OnRoundChanged_Client )

		Obituary_SetVerticalOffset( 90 )

		FlagInit( "WaitingForEvacObjective_Client" )
		FlagInit( "EvacObjective_Client" )
		FlagInit( "EvacShipArrived_Client" )
		FlagInit( "EvacShipDeparting_Client" )
		RegisterSignal( "StartingEvacObjectiveMessagingThread" )



		FlagInit( "AllianceAssignmentComplete" )
		if ( GetRespawnStyle() == eRespawnStyle.SPAWN_GROUP_SKYDIVE )
			SpawnGroupSkydive_SetCallback_GetSquadSpawnDelay( ShadowArmy_GetSpawnDelay )

		if ( AllianceProximity_ShouldOnlyDisplayPriorityPingsForAlliance() )
			AllianceProximity_SetPriorityPingsForAlliance( SHADOW_ARMY_ALLOWED_ALLIANCE_PINGS )

		RegisterCSVDialogue( $"datatable/dialogue/s19event_dialogue.rpak" )
		RegisterCommentaryBuckets( $"datatable/dialogue/s19event_dialogue.rpak" )
		RegisterDisabledBattleChatterEvents( SHADOWARMY_DISABLED_BATTLE_CHATTER_EVENTS )


}

void function ShGameMode_ShadowArmy_RegisterNetworking()
{
	if ( !IsShadowArmyGamemode() )
		return

	
	Remote_RegisterClientFunction( "ShadowArmy_ServerCallback_ShowAnnouncementMessage", "int", 0, eShadowArmyMessageIndex._count, "int", 0, eShadowArmyMessageType._count )
	Remote_RegisterClientFunction( "ShadowArmy_ServerCallback_ShowHinttMessage", "int", 0, eShadowArmyMessageIndex._count, "int", 0, INT_MAX, "int", 0, INT_MAX )
	Remote_RegisterClientFunction( "ShadowArmy_ServerCallback_GivePlayerRepeatingEnemyMapScans" )
	Remote_RegisterClientFunction( "ShadowArmy_ServerCallback_UpdateEvacTargetCountOnHud", "int", 0, MAX_EVAC_TARGET_COUNT_FOR_LEGENDS + 1 )
	Remote_RegisterClientFunction( "ShadowArmy_ServerCallback_SetDeathScreenCallbacks" )
	Remote_RegisterClientFunction( "ShadowArmy_ServerCallback_PlayMatchEndMusic", "bool" )
	Remote_RegisterClientFunction( "ShadowArmy_ServerCallback_SetAllianceAssignmentCompleteFlag" )
	Remote_RegisterClientFunction( "ShadowArmy_ServerCallback_DestroyLegendStartAreaMapFeature" )

	RegisterNetworkedVariable( "shadowArmy_PlayerForm", SNDC_PLAYER_GLOBAL, SNVT_UNSIGNED_INT, eShadowArmyRespawnForm.LIVING_LEGEND )
	RegisterNetworkedVariable( "shadowArmy_GamePhase", SNDC_GLOBAL, SNVT_UNSIGNED_INT, eShadowArmyGamePhase.GAME_START )
	RegisterNetworkedVariable( "shadowArmy_LegendsInEvacShips", SNDC_GLOBAL, SNVT_UNSIGNED_INT, 0 )
	RegisterNetworkedVariable( "shadowArmy_NextEvacPhaseTime", SNDC_GLOBAL, SNVT_TIME, -1 )


		RegisterNetVarIntChangeCallback ( "shadowArmy_GamePhase", OnShadowArmyGamePhaseChanged_Client )
		RegisterNetVarIntChangeCallback ( "shadowArmy_LegendsInEvacShips", UpdateLegendInEvacShipHUDCount )

}










bool function IsShadowArmyGamemode()
{
	return GetCurrentPlaylistVarBool( "is_shadow_army_gamemode", false )
}

bool function IsShadowArmyGamemodeCineVersion()
{
	return GetCurrentPlaylistVarBool( "is_shadow_army_cine_version", false )
}


const int UNSET_SQUAD_COUNT = -1
int function ShadowArmy_GetNumRevSquadsForMatchStart()
{
	
	string currentPlaylist = ShadowArmy_GetCurrentShadowArmyPlaylist()
	int squadCount = GetPlaylistVarInt( currentPlaylist, "shadow_army_starting_rev_squads", UNSET_SQUAD_COUNT )

	
	if ( squadCount < 0 )
	{
		int maxTeams = GetPlaylistVarInt( currentPlaylist, "max_teams", MAX_TEAMS )
		int maxAlliances = GetPlaylistVarInt( currentPlaylist, "max_alliances", 0 )

		if ( maxTeams > 0 && maxAlliances > 0 )
			squadCount = maxTeams / maxAlliances
		else
			squadCount = 0
	}

	return squadCount
}



const string DEFAULT_SHADOW_ARMY_PLAYLIST = "survival_shadow_army_base"
string function ShadowArmy_GetCurrentShadowArmyPlaylist()
{
	string currentPlaylist = GetCurrentPlaylistName()

	if ( currentPlaylist == "dev_default" )
		currentPlaylist = DEFAULT_SHADOW_ARMY_PLAYLIST

	return currentPlaylist
}






































int function GetNumLivingSquadsToTurnOffAllianceSwitch()
{
	return GetCurrentPlaylistVarInt( "shadow_army_legend_squad_count_to_end_switching", 1 )
}

int function GetNumLivingSquadsForEmergencyEvac()
{
	return GetCurrentPlaylistVarInt( "shadow_army_living_squad_count_emergency_evac", 3 )
}



































const int DEFAULT_STAGE_TO_REVEAL_EVAC_LOC = 3
int function GetRingStageWhenEvacLocationRevealed()
{
	return GetCurrentPlaylistVarInt( "shadow_army_ring_stage_to_reveal_evac_loc", DEFAULT_STAGE_TO_REVEAL_EVAC_LOC )
}











































































float function GetShadowRevBaseSpawnCooldown()
{
	return GetCurrentPlaylistVarFloat( "respawn_cooldown", SHADOWARMY_DEFAULT_SHORT_SPAWN_DELAY )
}




























































































































































































































































































































































































































void function ShadowArmy_ServerCallback_SetAllianceAssignmentCompleteFlag()
{
	FlagSet( "AllianceAssignmentComplete" )
}























void function ShadowArmy_OnRoundChanged_Client( int stage, float nextCircleStartTime )
{
	
	if ( stage == GetRingStageWhenEvacLocationRevealed() )
		UpdateMusicRampUpLevel( eShadowArmyMusicRampLevels.PRE_EVAC_SEQUENCE )
}





















































































































































































































































































array < entity > function ShadowArmy_GetEnemySquadPlayersForAllianceIntro( int alliance )
{
	int enemyAlliance = AllianceProximity_GetOtherAlliance( alliance )

	
	if ( enemyAlliance == SHADOWARMY_REVENANT_ALLIANCE && ShadowArmy_GetNumRevSquadsForMatchStart() <= 0 )
		return []

	array < int > populatedEnemySquads = AllianceProximity_GetPopulatedTeamsInAlliance( enemyAlliance )
	int finalSquad
	bool shouldSortFinalSquad = false

	
	if ( populatedEnemySquads.len() > 0 )
	{
		
		array < int > fullEnemySquads = []
		int maxTeamSize = GetMaxTeamSizeForPlaylist( GetCurrentPlaylistName() )
		foreach ( enemySquad in populatedEnemySquads )
		{
			if ( GetPlayerArrayOfTeam( enemySquad ).len() == maxTeamSize )
				fullEnemySquads.append( enemySquad )
		}

		
		array < int > finalEnemySquads = fullEnemySquads.len() > 0 ? fullEnemySquads : populatedEnemySquads

		
		if ( enemyAlliance == SHADOWARMY_LEGEND_ALLIANCE )
		{
			array < int > lobaSquads
			array < int > secondaryCharSquads
			foreach ( enemySquad in finalEnemySquads )
			{
				array < entity > enemySquadPlayers = GetPlayerArrayOfTeam( enemySquad )
				foreach ( enemyPlayer in enemySquadPlayers )
				{
					if ( ShadowArmy_IsPlayerLoba( enemyPlayer ) )
						lobaSquads.append( enemySquad )
					else if ( ShadowArmy_IsPlayerSecondaryStoryCharacter( enemyPlayer ) )
						secondaryCharSquads.append( enemySquad )
				}
			}

			if ( lobaSquads.len() > 0 )
			{
				finalEnemySquads = lobaSquads
				shouldSortFinalSquad = true
			}
			else if ( secondaryCharSquads.len() > 0 )
			{
				finalEnemySquads = secondaryCharSquads
				shouldSortFinalSquad = true
			}
		}

		
		
		
		finalSquad = finalEnemySquads[ 0 ]
	}
	else
	{
		
		array < int > teamsInAllianceArray = AllianceProximity_GetTeamsInAlliance( enemyAlliance )
		
		if ( teamsInAllianceArray.len() > 0 )
			finalSquad = teamsInAllianceArray[ 0 ]
		else
			return []
	}

	
	array < entity > finalSquadPlayers = GetPlayerArrayOfTeam( finalSquad )

	
	if ( shouldSortFinalSquad )
	{
		finalSquadPlayers.sort( int function( entity a, entity b ) : ( ) {
			
			bool aIsLoba = ShadowArmy_IsPlayerLoba( a )
			bool bIsLoba = ShadowArmy_IsPlayerLoba( b )

			if ( aIsLoba != bIsLoba )
			{
				if ( aIsLoba )
					return -1  

				if ( bIsLoba )
					return 1 
			}

			
			bool aIsSecondaryCharacter = ShadowArmy_IsPlayerSecondaryStoryCharacter( a )
			bool bIsSecondaryCharacter = ShadowArmy_IsPlayerSecondaryStoryCharacter( b )

			if ( aIsSecondaryCharacter != bIsSecondaryCharacter )
			{
				if ( aIsSecondaryCharacter )
					return -1  

				if ( bIsSecondaryCharacter )
					return 1 
			}

			return 0 
		} )
	}

	return finalSquadPlayers
}




bool function ShadowArmy_IsPlayerLoba( entity player )
{
	if ( !IsValid( player ) )
		return false

	ItemFlavor character = LoadoutSlot_GetItemFlavor( ToEHI( player ), Loadout_Character() )
	string characterRef  = ItemFlavor_GetCharacterRef( character ).tolower()

	if ( characterRef == "character_loba" )
		return true

	return false
}




bool function ShadowArmy_IsPlayerSecondaryStoryCharacter( entity player )
{
	if ( !IsValid( player ) )
		return false

	ItemFlavor character = LoadoutSlot_GetItemFlavor( ToEHI( player ), Loadout_Character() )
	string characterRef  = ItemFlavor_GetCharacterRef( character ).tolower()
	bool isSecondaryCharacter = false

	switch ( characterRef )
	{
		case "character_lifeline":
		case "character_valkyrie":
		case "character_madmaggie":
		case "character_crypto":
			isSecondaryCharacter = true
			break
		default:
			break
	}

	return isSecondaryCharacter
}

























































bool function ShadowArmy_IsPlayerOnShadowArmy( entity player )
{
	if ( !IsValid( player ) )
		return false

	if ( !player.IsPlayer() )
		return false

	return AllianceProximity_GetAllianceFromTeam( player.GetTeam() ) == SHADOWARMY_REVENANT_ALLIANCE
}




bool function ShadowArmy_ShouldEnterBleedout( entity player )
{
	if ( IsShadowArmyGamemode() && ShadowArmy_IsPlayerOnShadowArmy( player ) )
		return false

	return true
}




bool function ShadowArmy_IsPlayerInShadowForm( entity player )
{
	if ( !IsValid( player ) )
		return false

	return player.GetPlayerNetInt( "shadowArmy_PlayerForm" ) == eShadowArmyRespawnForm.SHADOW
}













































int function GetLivingLegendsCount()
{
	return AllianceProximity_GetNumPlayersInAlliance( SHADOWARMY_LEGEND_ALLIANCE, true )
}




int function GetLivingRevenantsCount()
{
	return AllianceProximity_GetNumPlayersInAlliance( SHADOWARMY_REVENANT_ALLIANCE, true )
}














int function ShadowArmy_GetCurrentGamePhase()
{
	return GetGlobalNetInt( "shadowArmy_GamePhase" )
}















































































































































































































































































































































































































































































































































































































float function ShadowArmy_GetSpawnDelay( int team )
{
	
	if ( AllianceProximity_GetAllianceFromTeam( team ) == SHADOWARMY_LEGEND_ALLIANCE )
		return GetCurrentPlaylistVarFloat( "shadow_army_short_respawn_cooldown", SHADOWARMY_DEFAULT_SHORT_SPAWN_DELAY )

	return GetShadowRevBaseSpawnCooldown()
}




int function ShadowArmy_GetLegendSpawnGroupsNumber()
{
	return GetCurrentPlaylistVarInt( "shadow_army_legend_spawn_groups_num", 3 )
}





















































































































































































































































































































void function ShadowArmy_OnTeamChangedAlliance_Client( int team, int newAlliance )
{
	entity localViewPlayer = GetLocalViewPlayer()

	if ( !IsValid( localViewPlayer ) )
		return

	
	
	
	array < entity > teamPlayers = GetPlayerArrayOfTeam( team )
	foreach ( teamPlayer in teamPlayers )
	{
		file.playersRespawningAfterAllianceSwitch.append( teamPlayer )
	}
}





































































































































































































































































































void function OnShadowArmyGamePhaseChanged_Client( entity player, int newGamePhase )
{
	switch ( newGamePhase )
	{
		case eShadowArmyGamePhase.GAME_START:
			break
		case eShadowArmyGamePhase.WAITING_FOR_EVAC_OBJECTIVE:
			FlagSet( "WaitingForEvacObjective_Client" )
			break
		case eShadowArmyGamePhase.EVAC_OBJECTIVE:
			FlagSet( "EvacObjective_Client" )
			UpdateMusicRampUpLevel( eShadowArmyMusicRampLevels.EVAC_SEQUENCE_STARTED )
			break
		case eShadowArmyGamePhase.EVAC_SHIP_ARRIVED:
			FlagSet( "EvacShipArrived_Client" )
			UpdateMusicRampUpLevel( eShadowArmyMusicRampLevels.EVAC_ARRIVED )
			break
		case eShadowArmyGamePhase.EVAC_SHIP_DEPARTED:
			FlagSet( "EvacShipDeparting_Client" )
			break
		default:
			return
	}
}










































void function ShadowArmy_ServerCallback_GivePlayerRepeatingEnemyMapScans()
{
	thread RunRepeatingEnemyMapScans_Thread()
}





const float POST_PULSE_WAIT = 1.0

const float SCAN_DURATION = 3.0

const float REPEAT_INTERVAL = 2.0

void function RunRepeatingEnemyMapScans_Thread()
{
#if DEV
		Assert( IsNewThread(), "Must be threaded off" )
#endif

	if ( IsValid( clGlobal.levelEnt ) )
		EndSignal( clGlobal.levelEnt, "OnDestroy" )

	
	if ( file.areEnemyMapScansActiveOnClient )
		return

	entity localPlayer = GetLocalClientPlayer()

	if ( !IsValid( localPlayer ) || !IsAlive( localPlayer ) )
		return

	EndSignal( localPlayer, "OnDestroy", "OnDeath" )

	int team = localPlayer.GetTeam()
	array< entity > aliveEnemies
	array< entity > scanEntsArray

	float endTime
	float timeToStartFade
	float timeToEndFade

	array<var> fullMapRuis
	array<var> minimapRuis
	array<entity> entsForTracking

	OnThreadEnd(
		function() : ( fullMapRuis, minimapRuis )
		{
			foreach( var ruiToDestroy in fullMapRuis )
			{
				Fullmap_RemoveRui( ruiToDestroy )
				RuiDestroyIfAlive( ruiToDestroy )
			}

			foreach( var ruiToDestroy in minimapRuis)
			{
				Minimap_CommonCleanup( ruiToDestroy )
			}

			file.areEnemyMapScansActiveOnClient = false
		}
	)

	file.areEnemyMapScansActiveOnClient = true

	while ( GetGameState() == eGameState.Playing )
	{
		
		vector pulseOrigin = localPlayer.GetOrigin()
		FullMap_PlayCryptoPulseSequence( pulseOrigin, true, SCAN_DURATION + POST_PULSE_WAIT )
		wait POST_PULSE_WAIT

		
		aliveEnemies = GetPlayerArrayOfEnemies_Alive( team )

		
		foreach ( enemy in aliveEnemies )
		{
			if ( !Bleedout_IsBleedingOut( enemy ) )
				scanEntsArray.append( enemy )
		}
		endTime = Time() + SCAN_DURATION
		timeToStartFade = Time() + ( SCAN_DURATION/2 )
		timeToEndFade = endTime

		
		foreach( entity enemy in scanEntsArray )
		{





			
			var fRui = FullMap_AddEnemyLocation( enemy )
			fullMapRuis.append( fRui )

			
			var mRui = Minimap_AddEnemyToMinimap( enemy )
			minimapRuis.append( mRui )
			RuiSetGameTime( mRui, "fadeStartTime", timeToStartFade )
			RuiSetGameTime( mRui, "fadeEndTime", timeToEndFade )
		}

		
		wait SCAN_DURATION

		
		foreach( var ruiToDestroy in fullMapRuis )
		{
			Fullmap_RemoveRui( ruiToDestroy )
			RuiDestroyIfAlive( ruiToDestroy )
		}

		foreach( var ruiToDestroy in minimapRuis)
		{
			Minimap_CommonCleanup( ruiToDestroy )
		}

		
		fullMapRuis.clear()
		minimapRuis.clear()
		aliveEnemies.clear()
		scanEntsArray.clear()

		
		wait REPEAT_INTERVAL
	}
}




































































































































































































































































































































































































































































































void function ObjectiveEvac_ManageHUDMessaging_Thread()
{
#if DEV
		Assert( IsNewThread(), "Must be threaded off" )
#endif

	entity localPlayer = GetLocalViewPlayer()
	if ( !IsValid( localPlayer ) )
		return

	
	localPlayer.Signal( "StartingEvacObjectiveMessagingThread" )
	EndSignal( localPlayer, "OnDestroy", "OnDeath", "StartingEvacObjectiveMessagingThread" )

	int currentPhase = ShadowArmy_GetCurrentGamePhase()
	var rui = ClGameState_GetRui()

	if ( currentPhase < eShadowArmyGamePhase.WAITING_FOR_EVAC_OBJECTIVE )
	{
		FlagWait( "WaitingForEvacObjective_Client" )
	}

	localPlayer = GetLocalViewPlayer()
	currentPhase = ShadowArmy_GetCurrentGamePhase()
	if ( currentPhase < eShadowArmyGamePhase.EVAC_OBJECTIVE )
	{
		if ( rui != null )
		{
			RuiSetBool( rui, "shouldShowEvacInfo", true )
			RuiSetBool( rui, "shouldShowEvacTimer", true )
			RuiSetColorAlpha( rui, "evacFrameColor", SrgbToLinear(< 1.0, 1.0, 1.0 >), 1.0 )
			RuiSetColorAlpha( rui, "evacIconColor", SrgbToLinear(< 1.0, 1.0, 1.0 >), 1.0 )
			RuiSetColorAlpha( rui, "evacBgColor", SrgbToLinear(< 0.1, 0.1, 0.1 >), 1.0 )
			RuiSetGameTime( rui, "nextEvacPhaseTime", GetGlobalNetTime( "shadowArmy_NextEvacPhaseTime" ) )
			if ( !ShadowArmy_IsPlayerOnShadowArmy( localPlayer ) )
			{
				RuiSetString( rui, "evacText", Localize( "#SHADOW_ARMY_OBJ_EVAC_SURVIVE" ) )
				RuiSetImage( rui, "evacIcon", $"rui/gamemodes/rev_army/timer_icon" )
			}
			else
			{
				RuiSetString( rui, "evacText", Localize( "#SHADOW_ARMY_OBJ_EVAC_ELIMINATE" ) )
				RuiSetImage( rui, "evacIcon", $"rui/gamemodes/rev_army/crosshair_icon" )
			}
		}

		FlagWait( "EvacObjective_Client" )

	}

	localPlayer = GetLocalViewPlayer()
	currentPhase = ShadowArmy_GetCurrentGamePhase()
	if ( currentPhase < eShadowArmyGamePhase.EVAC_SHIP_ARRIVED )
	{
		if ( rui != null )
		{
			RuiSetBool( rui, "shouldShowEvacInfo", true )
			RuiSetBool( rui, "shouldShowEvacTimer", true )
			RuiSetImage( rui, "evacIcon", $"rui/gamemodes/shadow_squad/evac_countdown" )
			RuiSetColorAlpha( rui, "evacFrameColor", SrgbToLinear( EVAC_INCOMING_HUD_ELEMENT_COL ), 1.0 )
			RuiSetColorAlpha( rui, "evacIconColor", SrgbToLinear( EVAC_INCOMING_HUD_ELEMENT_COL ), 1.0 )
			RuiSetColorAlpha( rui, "evacBgColor", SrgbToLinear(< 0.1, 0.1, 0.1 >), 1.0 )
			RuiSetGameTime( rui, "nextEvacPhaseTime", GetGlobalNetTime( "shadowArmy_NextEvacPhaseTime" ) )
			RuiSetString( rui, "evacText", Localize( "#SHADOW_ARMY_OBJ_EVAC_WAITING" ) )
		}

		FlagWait( "EvacShipArrived_Client" )
	}

	localPlayer = GetLocalViewPlayer()
	currentPhase = ShadowArmy_GetCurrentGamePhase()
	if ( currentPhase < eShadowArmyGamePhase.EVAC_SHIP_DEPARTED )
	{
		if ( rui != null )
		{
			RuiSetBool( rui, "shouldShowEvacInfo", true )
			RuiSetBool( rui, "shouldShowEvacTimer", true )
			RuiSetBool( rui, "hasEvacArrived", true )
			RuiSetImage( rui, "evacIcon", $"rui/gamemodes/shadow_squad/evac_countdown" )
			RuiSetColorAlpha( rui, "evacFrameColor", SrgbToLinear( EVAC_ARRIVED_HUD_ELEMENT_COL ), 1.0 )
			RuiSetColorAlpha( rui, "evacIconColor", SrgbToLinear(< 1.0, 1.0, 1.0 >), 1.0 )
			RuiSetColorAlpha( rui, "evacBgColor", SrgbToLinear(< 0.4, 0.15, 0.15>), 1.0 )
			RuiSetGameTime( rui, "nextEvacPhaseTime", GetGlobalNetTime( "shadowArmy_NextEvacPhaseTime" ) )
			RuiSetString( rui, "evacText", Localize( "#SHADOW_ARMY_OBJ_EVAC_ARRIVED" ) )
			RuiSetInt( rui, "legendsOnEvacShips", GetNumLegendsOnEvacShips())
			RuiSetInt( rui, "legendEvacTarget", file.evacTargetCount )
		}
		FlagWait( "EvacShipDeparting_Client" )
	}

	if ( rui != null )
	{
		RuiSetBool( rui, "shouldShowEvacInfo", false )
		RuiSetBool( rui, "shouldShowEvacTimer", false )
	}
}




















































































void function ShadowArmy_ServerCallback_SetDeathScreenCallbacks()
{
	DeathScreen_SetModeSpecificRuiUpdateFunc( ShadowArmy_DeathScreenUpdate )
	DeathScreen_SetDataRuiAssetForGamemode( DEATH_SCREEN_RUI )
}



void function ShadowArmy_DeathScreenUpdate( var rui )
{
	SquadSummaryData squadData = GetSquadSummaryData()

	string titleString = squadData.squadPlacement == 1 ? "#SQUAD_PLACEMENT_GCARDS_TITLE" : "#SHADOW_ARMY_SQUAD_HEADER_DEFEAT"
	string killsText   = "#CONTROL_DEATH_SCREEN_SUMMARY_KILLS_ALLIANCE"

	string victoryCondition = ShadowArmy_GetVictoryConditionForFlagset( squadData.gameResultFlags )
	RuiSetString( rui, "victoryCondition", victoryCondition )
	RuiSetString( rui, "headerText", titleString ) 
	RuiSetString( rui, "killsText", killsText )

	if ( squadData.gameResultFlags == SHADOWARMY_VICTORY_FLAGS_EVAC || squadData.gameResultFlags == SHADOWARMY_VICTORY_FLAGS_PREVENTED_EVAC )
	{
		
		RuiSetInt( rui, "losingScore", GetNumLegendsOnEvacShips() )
		RuiSetInt( rui, "winningScore", file.evacTargetCount )
	}
}



string function ShadowArmy_GetVictoryConditionForFlagset( int gameResultFlags )
{
	string victoryCondition
	switch( gameResultFlags )
	{
		case( SHADOWARMY_VICTORY_FLAGS_ELIMINATION ):
			victoryCondition = SHADOWARMY_PIN_VICTORYCONDITION_ELIMINATION
			break
		case( SHADOWARMY_VICTORY_FLAGS_EVAC ):
			victoryCondition = SHADOWARMY_PIN_VICTORYCONDITION_EVAC
			break
		case( SHADOWARMY_VICTORY_FLAGS_PREVENTED_EVAC ):
			victoryCondition = SHADOWARMY_PIN_VICTORYCONDITION_PREVENTED_EVAC
			break
		case( SHADOWARMY_VICTORY_FLAGS_FORFEIT ):
			victoryCondition = SHADOWARMY_PIN_VICTORYCONDITION_FORFEIT
			break
		default:
			victoryCondition = SHADOWARMY_PIN_VICTORYCONDITION_UNKNOWN
			break
	}

	return victoryCondition
}

























































const array < string > ELIMINATION_DIALOGUE_LINES = [ "diag_ap_nocNotify_ltm_evacFail_01_01_3p",
	"diag_ap_nocNotify_ltm_evacFail_01_02_3p",
	"diag_ap_nocNotify_ltm_evacFail_02_01_3p",
	"diag_ap_nocNotify_ltm_evacFail_02_02_3p",
	"diag_ap_nocNotify_ltm_evacFail_03_01_3p",
	"diag_ap_nocNotify_ltm_evacFail_03_02_3p",
	"diag_ap_nocNotify_ltm_evacFail_04_01_3p",
	"diag_ap_nocNotify_ltm_evacFail_04_02_3p"
]
const array < string > EVAC_SUCCESS_DIALOGUE_LINES = [ "diag_ap_nocNotify_ltm_evacComplete_01_01_3p",
	"diag_ap_nocNotify_ltm_evacComplete_01_02_3p",
	"diag_ap_nocNotify_ltm_evacComplete_02_01_3p",
	"diag_ap_nocNotify_ltm_evacComplete_02_02_3p",
	"diag_ap_nocNotify_ltm_evacComplete_03_01_3p",
	"diag_ap_nocNotify_ltm_evacComplete_03_02_3p",
	"diag_ap_nocNotify_ltm_evacComplete_04_01_3p",
	"diag_ap_nocNotify_ltm_evacComplete_04_02_3p",
	"diag_ap_nocNotify_ltm_evacComplete_05_01_3p",
	"diag_ap_nocNotify_ltm_evacComplete_05_02_3p",
]
const array < string > EVAC_FAIL_DIALOGUE_LINES = [ "diag_ap_nocNotify_ltm_evacNearMiss_01_01_3p",
	"diag_ap_nocNotify_ltm_evacNearMiss_01_02_3p",
	"diag_ap_nocNotify_ltm_evacNearMiss_02_01_3p",
	"diag_ap_nocNotify_ltm_evacNearMiss_02_02_3p",
	"diag_ap_nocNotify_ltm_evacNearMiss_03_01_3p",
	"diag_ap_nocNotify_ltm_evacNearMiss_03_02_3p",
	"diag_ap_nocNotify_ltm_evacNearMiss_04_01_3p",
	"diag_ap_nocNotify_ltm_evacNearMiss_04_02_3p",
]
VictorySoundPackage function GetVictorySoundPackage()
{
	VictorySoundPackage victorySoundPackage
	SquadSummaryData winningSquadData = GetWinnerSquadSummaryData()

	switch( winningSquadData.gameResultFlags )
	{
		case( SHADOWARMY_VICTORY_FLAGS_ELIMINATION ):
			victorySoundPackage.youAreChampPlural = ELIMINATION_DIALOGUE_LINES.getrandom()
			victorySoundPackage.youAreChampSingular = ELIMINATION_DIALOGUE_LINES.getrandom()
			victorySoundPackage.theyAreChampSingular = ELIMINATION_DIALOGUE_LINES.getrandom()
			victorySoundPackage.theyAreChampPlural = ELIMINATION_DIALOGUE_LINES.getrandom()
			break
		case( SHADOWARMY_VICTORY_FLAGS_EVAC ):
			victorySoundPackage.youAreChampPlural = EVAC_SUCCESS_DIALOGUE_LINES.getrandom()
			victorySoundPackage.youAreChampSingular = EVAC_SUCCESS_DIALOGUE_LINES.getrandom()
			victorySoundPackage.theyAreChampSingular = EVAC_SUCCESS_DIALOGUE_LINES.getrandom()
			victorySoundPackage.theyAreChampPlural = EVAC_SUCCESS_DIALOGUE_LINES.getrandom()
			break
		case( SHADOWARMY_VICTORY_FLAGS_PREVENTED_EVAC ):
			victorySoundPackage.youAreChampPlural = EVAC_FAIL_DIALOGUE_LINES.getrandom()
			victorySoundPackage.youAreChampSingular = EVAC_FAIL_DIALOGUE_LINES.getrandom()
			victorySoundPackage.theyAreChampSingular = EVAC_FAIL_DIALOGUE_LINES.getrandom()
			victorySoundPackage.theyAreChampPlural = EVAC_FAIL_DIALOGUE_LINES.getrandom()
			break
		case( SHADOWARMY_VICTORY_FLAGS_FORFEIT ):
			victorySoundPackage.youAreChampPlural = "diag_ap_nocNotify_ltm_evacFail_04_01_3p"
			victorySoundPackage.youAreChampSingular = "diag_ap_nocNotify_ltm_evacFail_04_01_3p"
			victorySoundPackage.theyAreChampSingular = "diag_ap_nocNotify_ltm_evacComplete_01_01_3p"
			victorySoundPackage.theyAreChampPlural = "diag_ap_nocNotify_ltm_evacComplete_01_01_3p"
			break
		default: 
			victorySoundPackage.youAreChampPlural = "diag_ap_nocNotify_ltm_evacFail_04_01_3p"
			victorySoundPackage.youAreChampSingular = "diag_ap_nocNotify_ltm_evacFail_04_01_3p"
			victorySoundPackage.theyAreChampSingular = "diag_ap_nocNotify_ltm_evacComplete_01_01_3p"
			victorySoundPackage.theyAreChampPlural = "diag_ap_nocNotify_ltm_evacComplete_01_01_3p"
			break
	}

	return victorySoundPackage
}




const float SFX_DELAY = 2.5
void function ShadowArmy_PlayIntroBannerSoundForLegends_Thread()
{
#if DEV
		Assert( IsNewThread(), "Must be threaded off" )
#endif

	if ( IsValid( clGlobal.levelEnt ) )
		EndSignal( clGlobal.levelEnt, "OnDestroy" )

	wait SFX_DELAY

	EmitUISound( SFX_SHADOWARMY_LEGEND_SPAWN )
}




int function GetNumLegendsOnEvacShips()
{
	return GetGlobalNetInt( "shadowArmy_LegendsInEvacShips" )
}




void function UpdateLegendInEvacShipHUDCount( entity player, int newCount )
{
	var rui = ClGameState_GetRui()

	if ( rui != null )
		RuiSetInt( rui, "legendsOnEvacShips", newCount )
}




void function ShadowArmy_ServerCallback_UpdateEvacTargetCountOnHud( int targetCount )
{
	if ( targetCount >= 0 && targetCount <= MAX_EVAC_TARGET_COUNT_FOR_LEGENDS )
	{
		file.evacTargetCount = targetCount
		var rui = ClGameState_GetRui()
		if ( rui != null )
			RuiSetInt( rui, "legendEvacTarget", targetCount )
	}
}
















































































































































































































































































































































void function CreateMusicEntityIfNotValid()
{
	if ( !IsValid( file.musicEntity ) )
	{
		file.musicEntity = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", <0,0,10000>, <0, 0, 0> )
		file.musicRampUpLevel = MUSIC_RAMPUP_LEVEL_NOT_SET
	}
}



void function UpdateMusicRampUpLevel( int newLevel )
{
	entity localPlayer = GetLocalClientPlayer()

	if ( !IsValid( localPlayer ) )
		return

	CreateMusicEntityIfNotValid()
	if ( !IsValid( file.musicEntity ) )
		return

	
	if ( newLevel > file.musicRampUpLevel && newLevel < MUSIC_RAMPUP_CONTROLLER_VALUES.len() )
	{
		
		if ( file.musicRampUpLevel == MUSIC_RAMPUP_LEVEL_NOT_SET )
		{
			if ( ShadowArmy_IsPlayerOnShadowArmy( localPlayer ) )
				EmitSoundOnEntity( file.musicEntity, MUSIC_SHADOWARMY_GAMEPLAY_RAMPUP_REV )
			else
				EmitSoundOnEntity( file.musicEntity, MUSIC_SHADOWARMY_GAMEPLAY_RAMPUP_LEGEND )

			file.musicEntity.UnsetSoundCodeControllerValue()
		}

		float controllerValue = MUSIC_RAMPUP_CONTROLLER_VALUES[ newLevel ]
		file.musicEntity.SetSoundCodeControllerValue( controllerValue )
		file.musicRampUpLevel = newLevel
	}
}





void function ResetMusicRampUpAtLevelForClient()
{
	
	if ( GetGameState() != eGameState.Playing )
		return

	entity localPlayer = GetLocalClientPlayer()

	if ( !IsValid( localPlayer ) )
		return

	
	ShadowArmy_StopRampUpMusic()

	
	int currentGamePhase = ShadowArmy_GetCurrentGamePhase()
	if ( currentGamePhase == eShadowArmyGamePhase.EVAC_SHIP_ARRIVED )
		UpdateMusicRampUpLevel( eShadowArmyMusicRampLevels.EVAC_ARRIVED )
	else if ( currentGamePhase == eShadowArmyGamePhase.EVAC_OBJECTIVE )
		UpdateMusicRampUpLevel( eShadowArmyMusicRampLevels.EVAC_SEQUENCE_STARTED )
	else if ( currentGamePhase < eShadowArmyGamePhase.EVAC_SHIP_DEPARTED && SURVIVAL_GetCurrentDeathFieldStage() >= GetRingStageWhenEvacLocationRevealed() )
		UpdateMusicRampUpLevel( eShadowArmyMusicRampLevels.PRE_EVAC_SEQUENCE )
}




void function ShadowArmy_ServerCallback_PlayMatchEndMusic( bool didLegendsWin )
{
	entity localPlayer = GetLocalClientPlayer()

	if ( !IsValid( localPlayer ) )
		return

	bool isPlayerOnLegendAlliance = !ShadowArmy_IsPlayerOnShadowArmy( localPlayer )
	bool didLocalAllianceWin = isPlayerOnLegendAlliance && didLegendsWin || !isPlayerOnLegendAlliance && !didLegendsWin

	
	ShadowArmy_StopRampUpMusic()

	
	if ( didLocalAllianceWin )
		EmitUISound( SFX_SHADOWARMY_MATCH_WIN_STINGER )
	else
		EmitUISound( SFX_SHADOWARMY_MATCH_LOSE_STINGER )

	
	if ( didLegendsWin )
		EmitSoundOnEntity( localPlayer, MUSIC_SHADOWARMY_LEGEND_VICTORY )
	else
		EmitSoundOnEntity( localPlayer, MUSIC_SHADOWARMY_REV_VICTORY )
}




void function ShadowArmy_StopRampUpMusic()
{
	if ( IsValid( file.musicEntity ) )
	{
		StopSoundOnEntity( file.musicEntity, MUSIC_SHADOWARMY_GAMEPLAY_RAMPUP_LEGEND )
		StopSoundOnEntity( file.musicEntity, MUSIC_SHADOWARMY_GAMEPLAY_RAMPUP_REV )
	}
	file.musicRampUpLevel = MUSIC_RAMPUP_LEVEL_NOT_SET
}
























void function OverrideGameState()
{
	ClGameState_RegisterGameStateAsset( $"ui/gamestate_survival_shadowarmy.rpak" )
}

























































































































































void function ShadowArmy_ServerCallback_ShowAnnouncementMessage( int messageIndex, int messageType )
{
	ShowAnnouncementMessage( messageIndex, messageType )
}




void function ShowAnnouncementMessage( int messageIndex, int messageType )
{
	if ( messageIndex < 0 || eShadowArmyMessageIndex._count < messageIndex )
		return

	entity player = GetLocalClientPlayer()
	if ( !IsValid( player ) )
		return

	string messageText
	string subText
	string obitText
	asset leftIcon
	asset rightIcon
	string soundAlias = SFX_HUD_ANNOUNCE_QUICK
	float duration = SHADOWARMY_ANNOUNCEMENT_DURATION
	vector titleColor

	switch( messageIndex )
	{
		case eShadowArmyMessageIndex.BLANK:
			messageText = ""
			subText = ""
			obitText = ""
			duration = 0.0
			break
		case eShadowArmyMessageIndex.LTM_DROP_ANNOUNCE_LEGEND:
			messageText = GetCurrentPlaylistVarString( "name", "#GAMEMODE_ANNOUNCE_NONE" )
			subText = "#SHADOW_ARMY_LTM_BANNER_MESSAGE_LEGEND"
			soundAlias = SFX_HUD_ANNOUNCE_LTM
			leftIcon = ANNOUNCEMENT_LOBA_ICON_FG
			rightIcon = ANNOUNCEMENT_LOBA_ICON_FG
			titleColor = GetKeyColor( COLORID_ALLIANCE_0 )
			break
		case eShadowArmyMessageIndex.LTM_DROP_ANNOUNCE_SHADOW:
			messageText = GetCurrentPlaylistVarString( "name", "#GAMEMODE_ANNOUNCE_NONE" )
			subText = "#SHADOW_ARMY_LTM_BANNER_MESSAGE_REV"
			duration = ANNOUNCEMENT_DURATION
			soundAlias = SFX_HUD_ANNOUNCE_LTM
			leftIcon = ANNOUNCEMENT_SHADOW_REV_ICON_FG
			rightIcon = ANNOUNCEMENT_SHADOW_REV_ICON_FG
			titleColor = GetKeyColor( COLORID_ALLIANCE_1 )
			break
		case eShadowArmyMessageIndex.RESPAWNING_AS_SHADOW_FIRST_TIME:
			messageText = "#SHADOW_ARMY_RESPAWNING_REV_FIRST_TIME"
			subText = "#SHADOW_ARMY_RESPAWNING_REV_FIRST_TIME_SUB"
			soundAlias = SFX_SHADOWARMY_REV_ARMY_REV_RESPAWN_STINGER
			leftIcon = ANNOUNCEMENT_SHADOW_REV_ICON_FG
			rightIcon = ANNOUNCEMENT_SHADOW_REV_ICON_FG
			titleColor = GetKeyColor( COLORID_ALLIANCE_1 )
			break
		case eShadowArmyMessageIndex.RESPAWNING_AS_SHADOW:
			messageText = "#SHADOW_ARMY_RESPAWNING_REV"
			subText = "#SHADOW_ARMY_RESPAWNING_REV_SUB"
			soundAlias = SFX_SHADOWARMY_REV_ARMY_REV_RESPAWN_STINGER
			leftIcon = ANNOUNCEMENT_SHADOW_REV_ICON_FG
			rightIcon = ANNOUNCEMENT_SHADOW_REV_ICON_FG
			titleColor = GetKeyColor( COLORID_ALLIANCE_1 )
			break
		case eShadowArmyMessageIndex.RESPAWNING_AS_SHADOW_FROM_LEGEND:
			messageText = "#SHADOW_ARMY_RESPAWNING_REV_FROM_LEGEND"
			subText = "#SHADOW_ARMY_RESPAWNING_REV_FROM_LEGEND_SUB"
			soundAlias = MUSIC_SHADOWARMY_LEGEND_SPAWN_AS_REV
			leftIcon = ANNOUNCEMENT_SHADOW_REV_ICON_FG
			rightIcon = ANNOUNCEMENT_SHADOW_REV_ICON_FG
			titleColor = GetKeyColor( COLORID_ALLIANCE_1 )
			ResetMusicRampUpAtLevelForClient() 
			break
		case eShadowArmyMessageIndex.RESPAWNING_AS_FULL_REV:
			messageText = "#SHADOW_ARMY_RESPAWNING_FULL_REV"
			subText = "#SHADOW_ARMY_RESPAWNING_FULL_REV_SUB"
			soundAlias = SFX_SHADOWARMY_REDEYE_REV_RESPAWN_STINGER
			leftIcon = ANNOUNCEMENT_FULL_REV_ICON_FG
			rightIcon = ANNOUNCEMENT_FULL_REV_ICON_FG
			titleColor = ANNOUNCEMENT_FULL_REV_BANNER_COLOR
			break
		case eShadowArmyMessageIndex.RESPAWNING_AS_LEGEND:
			messageText = "#SHADOW_ARMY_RESPAWNING_LEGEND"
			subText = "#SHADOW_ARMY_RESPAWNING_LEGEND_SUB"
			leftIcon = ANNOUNCEMENT_LOBA_ICON_FG
			rightIcon = ANNOUNCEMENT_LOBA_ICON_FG
			titleColor = GetKeyColor( COLORID_ALLIANCE_0 )
			break
		case eShadowArmyMessageIndex.RESPAWNING_AS_LEGEND_EVAC:
			messageText = "#SHADOW_ARMY_RESPAWNING_LEGEND_EVAC"
			subText = "#SHADOW_ARMY_RESPAWNING_LEGEND_EVAC_SUB"
			leftIcon = ANNOUNCEMENT_LOBA_ICON_FG
			rightIcon = ANNOUNCEMENT_LOBA_ICON_FG
			titleColor = GetKeyColor( COLORID_ALLIANCE_0 )
			break
		case eShadowArmyMessageIndex.FULL_REV_SPAWNED_LEGEND:
			messageText = "#SHADOW_ARMY_FULL_REV_SPAWNED_LEGEND"
			subText = "#SHADOW_ARMY_FULL_REV_SPAWNED_LEGEND_SUB"
			obitText = "#SHADOW_ARMY_FULL_REV_SPAWNED_OBIT"
			soundAlias = SFX_SHADOWARMY_REVENGE_KILL_MSG
			leftIcon = ANNOUNCEMENT_FULL_REV_ICON_FG
			rightIcon = ANNOUNCEMENT_FULL_REV_ICON_FG
			titleColor = ANNOUNCEMENT_FULL_REV_BANNER_COLOR
			break
		case eShadowArmyMessageIndex.FULL_REV_SPAWNED_SHADOW:
			messageText = "#SHADOW_ARMY_FULL_REV_SPAWNED_SHADOW"
			subText = "#SHADOW_ARMY_FULL_REV_SPAWNED_SHADOW_SUB"
			obitText = "#SHADOW_ARMY_FULL_REV_SPAWNED_OBIT"
			leftIcon = ANNOUNCEMENT_FULL_REV_ICON_FG
			rightIcon = ANNOUNCEMENT_FULL_REV_ICON_FG
			titleColor = ANNOUNCEMENT_FULL_REV_BANNER_COLOR
			break
		case eShadowArmyMessageIndex.FULL_REV_KILLED:
			obitText = "#SHADOW_ARMY_FULL_REV_KILLED_OBIT"
			break
		case eShadowArmyMessageIndex.LEGEND_TEAM_SWITCHED_TO_REV:
			obitText = "#SHADOW_ARMY_LEGENDS_SWITCH_OBIT"
			break
		case eShadowArmyMessageIndex.EVAC_CALLED_IN_RING_LEGEND:
			messageText = "#SHADOW_ARMY_EVAC_CALLED_IN_RING_LEGEND"
			subText = "#SHADOW_ARMY_EVAC_CALLED_IN_LEGEND_SUB"
			soundAlias = SFX_SHADOWARMY_EVAC_MSG_STINGER_NORMAL
			titleColor = SrgbToLinear( EVAC_INCOMING_HUD_ELEMENT_COL )
			break
		case eShadowArmyMessageIndex.EVAC_CALLED_IN_RING_SHADOW:
			messageText = "#SHADOW_ARMY_EVAC_CALLED_IN_RING_SHADOW"
			subText = "#SHADOW_ARMY_EVAC_CALLED_IN_SHADOW_SUB"
			soundAlias = SFX_SHADOWARMY_EVAC_MSG_STINGER_NORMAL
			titleColor = SrgbToLinear( EVAC_INCOMING_HUD_ELEMENT_COL )
			break
		case eShadowArmyMessageIndex.EVAC_CALLED_IN_EMERGENCY_LEGEND:
			messageText = "#SHADOW_ARMY_EVAC_CALLED_IN_EMERG_LEGEND"
			subText = "#SHADOW_ARMY_EVAC_CALLED_IN_LEGEND_SUB"
			soundAlias = SFX_SHADOWARMY_EVAC_MSG_STINGER_NORMAL
			titleColor = SrgbToLinear( EVAC_INCOMING_HUD_ELEMENT_COL )
			break
		case eShadowArmyMessageIndex.EVAC_CALLED_IN_EMERGENCY_SHADOW:
			messageText = "#SHADOW_ARMY_EVAC_CALLED_IN_EMERG_SHADOW"
			subText = "#SHADOW_ARMY_EVAC_CALLED_IN_SHADOW_SUB"
			soundAlias = SFX_SHADOWARMY_EVAC_MSG_STINGER_NORMAL
			titleColor = SrgbToLinear( EVAC_INCOMING_HUD_ELEMENT_COL )
			break
		case eShadowArmyMessageIndex.EVAC_ARRIVED_LEGEND:
			messageText = "#SHADOW_ARMY_EVAC_ARRIVED_LEGEND"
			subText = "#SHADOW_ARMY_EVAC_ARRIVED_LEGEND_SUB"
			titleColor = SrgbToLinear( EVAC_ARRIVED_HUD_ELEMENT_COL )
			break
		case eShadowArmyMessageIndex.EVAC_ARRIVED_SHADOW:
			messageText = "#SHADOW_ARMY_EVAC_ARRIVED_SHADOW"
			subText = "#SHADOW_ARMY_EVAC_ARRIVED_SHADOW_SUB"
			titleColor = SrgbToLinear( EVAC_ARRIVED_HUD_ELEMENT_COL )
			break
		case eShadowArmyMessageIndex.SAFE_ON_EVAC_SHIP:
			messageText = "#SHADOW_ARMY_SAFE_ON_EVAC_SHIP"
			subText = "#SHADOW_ARMY_SAFE_ON_EVAC_SHIP_SUB"
			soundAlias = SFX_SHADOWARMY_END_MSG
			leftIcon = ANNOUNCEMENT_LOBA_ICON_FG
			rightIcon = ANNOUNCEMENT_LOBA_ICON_FG
			titleColor = GetKeyColor( COLORID_ALLIANCE_0 )
			break
		case eShadowArmyMessageIndex.ALLIANCE_SWITCH_DISABLED_LEGEND:
			messageText = "#SHADOW_ARMY_SWITCHING_DISABLED_LEGEND"
			subText = "#SHADOW_ARMY_SWITCHING_DISABLED_LEGEND_SUB"
			obitText = "#SHADOW_ARMY_LEGENDS_SWITCH_DISABLED_OBIT"
			soundAlias = SFX_SHADOWARMY_END_MSG
			titleColor = GetKeyColor( COLORID_ALLIANCE_0 )
			break
		case eShadowArmyMessageIndex.ALLIANCE_SWITCH_DISABLED_SHADOW:
			messageText = "#SHADOW_ARMY_SWITCHING_DISABLED_SHADOW"
			subText = "#SHADOW_ARMY_SWITCHING_DISABLED_SHADOW_SUB"
			obitText = "#SHADOW_ARMY_LEGENDS_SWITCH_DISABLED_OBIT"
			soundAlias = SFX_SHADOWARMY_END_MSG
			titleColor = GetKeyColor( COLORID_ALLIANCE_1 )
			break
		case eShadowArmyMessageIndex.LEGENDS_RESPAWNED:
			obitText = "#SHADOW_ARMY_LEGENDS_RESPAWNED_OBIT"
			break
		case eShadowArmyMessageIndex.LEGEND_ENTERED_EVAC_SHIP:
			obitText = "#SHADOW_ARMY_LEGEND_BOARDED_EVAC"
			break
		case eShadowArmyMessageIndex.EVAC_AREA_UPDATED:
			if ( !ShadowArmy_IsPlayerOnShadowArmy( player ) )
				obitText = "#SHADOW_ARMY_EVAC_LOCATION_UPDATED"
			else
				return 
			break
		case eShadowArmyMessageIndex.EVAC_LOCATION_REVEALED:
			obitText = "#SHADOW_ARMY_EVAC_LOCATION_REVEALED"
			break
		default:
#if DEV
				Assert( false, "Shadow Army: Unhandled messageIndex: " + messageIndex )
#endif
			break
	}

	if ( messageType == eShadowArmyMessageType.ANNOUNCE_ONLY || messageType == eShadowArmyMessageType.ANNOUNCE_AND_OBIT )
	{
		AnnouncementData announcement = Announcement_Create( messageText )
		Announcement_SetSubText( announcement, subText )
		Announcement_SetStyle( announcement, ANNOUNCEMENT_STYLE_SWEEP )
		Announcement_SetPurge( announcement, true )
		Announcement_SetPriority( announcement, 200 )
		Announcement_SetSoundAlias( announcement, soundAlias )
		announcement.duration = duration
		Announcement_SetLeftIcon( announcement, leftIcon )
		Announcement_SetRightIcon( announcement, rightIcon )
		Announcement_SetTitleColor( announcement, titleColor )
		AnnouncementFromClass( player, announcement )
	}

	if ( messageType == eShadowArmyMessageType.OBIT_ONLY || messageType == eShadowArmyMessageType.ANNOUNCE_AND_OBIT )
		Obituary_Print_Localized( Localize( obitText ) )

	
	if ( messageIndex == eShadowArmyMessageIndex.LEGENDS_RESPAWNED )
		UpdateLegendsWaitingToRespawnCountOnHud()
}





const float HINT_DISPLAY_TIME = 5.0
const float HINT_FADE_TIME = 1.0
void function ShadowArmy_ServerCallback_ShowHinttMessage( int hintIndex, int optionalInt1, int optionalInt2 )
{
	if ( hintIndex < 0 || eShadowArmyHintIndex._count < hintIndex )
		return

	entity player = GetLocalClientPlayer()
	if ( !IsValid( player ) )
		return

	string messageText
	bool shouldDisplayHint = true

	switch( hintIndex )
	{
		case eShadowArmyHintIndex.FULL_REV_CANDIDATE_HINT:
			string buttonHint = IsControllerModeActive() ? "%offhand1% + %ping%" : "%offhand4%"
			messageText = Localize( "#SHADOW_ARMY_FULL_REV_CANDIDATE_HINT", buttonHint )
			break
		case eShadowArmyHintIndex.FULL_REV_CRITERIA_NO_DAM_HINT:
			messageText = "#SHADOW_ARMY_FULL_REV_CRITERIA_NO_DAM_HINT"
			break
		default:
#if DEV
				Assert( false, "Shadow Army: Unhandled hintIndex: " + hintIndex )
#endif
			shouldDisplayHint = false
			break
	}

	if ( shouldDisplayHint )
		AddPlayerHint( HINT_DISPLAY_TIME, HINT_FADE_TIME, $"", messageText )
}



void function OnPlayerLifeStateChanged_Client( entity player, int oldState, int newState )
{
	UpdateLegendsWaitingToRespawnCountOnHud()

	
	
	if ( IsValid( player ) && IsAlive( player ) && Flag( "AllianceAssignmentComplete" ) && file.playersRespawningAfterAllianceSwitch.contains( player ) )
	{
		thread UpdatePlayerHUDOnDelay_Thread( player )
		file.playersRespawningAfterAllianceSwitch.fastremovebyvalue( player )
	}
}




const float HUD_UPDATE_DELAY = 0.5
void function UpdatePlayerHUDOnDelay_Thread( entity player )
{
#if DEV
		Assert( IsNewThread(), "Must be threaded off" )
#endif

	if ( IsValid( clGlobal.levelEnt ) )
		EndSignal( clGlobal.levelEnt, "OnDestroy" )

	if ( !IsValid( player ) )
		return

	EndSignal( player, "OnDestroy" )

	wait HUD_UPDATE_DELAY

	Squads_SetCustomPlayerInfo( player )
	ShadowArmy_SetGameStateIsRevArmy( player )
}




void function ShadowArmy_OnGamestateEnterPlaying_Client()
{
	thread RunPostAllianceAssignmentCompleteLogic_Thread( GetLocalClientPlayer() )
}



void function OnYouRespawned()
{
	entity localPlayer = GetLocalViewPlayer()
	if ( IsValid( localPlayer ) && localPlayer.IsPlayer() )
	{
		
		if ( GetGameState() == eGameState.Playing && Flag( "AllianceAssignmentComplete" ) )
		{
			array < entity > allPlayers = GetPlayerArray()
			foreach ( entity player in allPlayers )
			{
				if ( !IsValid( player ) )
					continue

				Squads_SetCustomPlayerInfo( player )
			}
		}

		thread ObjectiveEvac_ManageHUDMessaging_Thread()
	}
}



void function ShadowArmy_OnPlayerConnectionStateChanged( entity player )
{
	if ( IsValid( player ) && player == GetLocalClientPlayer() )
	{
		
		if ( player.IsConnectionActive() )
		{
			thread ObjectiveEvac_ManageHUDMessaging_Thread()
			ResetMusicRampUpAtLevelForClient()
		}
	}
}




const int EVAC_AREA_ICON_PRIORITY = 1100
void function RunPostAllianceAssignmentCompleteLogic_Thread( entity localPlayer )
{
#if DEV
		Assert( IsNewThread(), "Must be threaded off" )
#endif

	if ( IsValid( clGlobal.levelEnt ) )
		EndSignal( clGlobal.levelEnt, "OnDestroy" )

	if ( !IsValid( localPlayer ) )
		return

	EndSignal( localPlayer, "OnDestroy" )

	if ( !Flag( "AllianceAssignmentComplete" ) )
		FlagWait( "AllianceAssignmentComplete" )

	UpdateLegendsWaitingToRespawnCountOnHud()

	
	SetMapFeatureItem( EVAC_AREA_ICON_PRIORITY, "#SHADOW_ARMY_EVAC_AREA", "#SHADOW_ARMY_EVAC_AREA_DESC", EVAC_AREA_ICON )

	
	
	if ( ShadowArmy_IsPlayerOnShadowArmy( localPlayer ) )
		SetMapFeatureItem( EVAC_AREA_ICON_PRIORITY, "#SHADOW_ARMY_LEGEND_SPAWN_AREA", "#SHADOW_ARMY_LEGEND_SPAWN_AREA_DESC", LEGEND_START_AREA_ICON )

	array < entity > allPlayers = GetPlayerArray()
	foreach ( entity player in allPlayers )
	{
		if ( !IsValid( player ) )
			continue

		Squads_SetCustomPlayerInfo( player )
		ShadowArmy_SetGameStateIsRevArmy( player )
	}
}



void function ShadowArmy_OnSpectateTargetChanged( entity player, entity previousTarget, entity currentTarget )
{
	if ( IsValid( currentTarget ) && currentTarget.IsPlayer() && GetGameState() == eGameState.Playing )
	{
		thread RunPostAllianceAssignmentCompleteLogic_Thread( player )
		thread ObjectiveEvac_ManageHUDMessaging_Thread()
	}
}



void function ShadowArmy_SetGameStateIsRevArmy( entity player )
{
	if( !IsValid( player ) )
		return

	var rui = ClGameState_GetRui()

	if ( rui != null )
		RuiSetBool( rui, "isRevArmy", ShadowArmy_IsPlayerOnShadowArmy( player ) )
}




void function UpdateLegendsWaitingToRespawnCountOnHud()
{
	var rui = ClGameState_GetRui()

	if ( rui != null )
		RuiSetInt( rui, "legendsAwaitingRespawn", ShadowArmy_RespawnBeacon_GetLegendsWaitingToRespawnCount() )

	
	ShadowArmy_RespawnBeacon_UpdateBeaconMapFeature()
}






























































































































































































void function ShadowArmy_ServerCallback_DestroyLegendStartAreaMapFeature()
{
	RemoveMapFeatureItemByName( "#SHADOW_ARMY_LEGEND_SPAWN_AREA" )
}





















































































































































                                  