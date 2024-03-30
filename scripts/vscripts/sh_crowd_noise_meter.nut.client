global function CrowdNoiseMeter_Init
global function CrowdNoiseMeter_RegisterNetworking
global function CrowdNoiseMeterEnabled


















global function CrowdNoiseMeter_PlayGameEndSound
global function ServerCallback_CrowdNoiseMeterValueUpdate
global function SetCallback_CrowdNoiseMeter_OnFirstSpawn
global function TriggerCrowdEventOnEntity
global function ToggleCrowdSoundOnEntity



#if DEV
global function DEV_Cl_ToggleCrowdNoiseMeterOnPlayer
global function DEV_Cl_ToggleCrowdNoiseMeterOnServer
global function DEV_Cl_ToggleCrowdNoiseMeterDebug
#endif


global enum eCrowdNoiseMeterModifiers
{
	CONTROL_HALFWAY_POINT_POSITIVE,
	CONTROL_HALFWAY_POINT_NEGATIVE,
	CONTROL_HUGE_LEAD_POSITIVE,
	CONTROL_HUGE_LEAD_NEGATIVE,
	CONTROL_IMMINENT_WIN_POSITIVE,
	CONTROL_IMMINENT_WIN_NEGATIVE,
	CONTROL_LOCKOUT_POSITIVE,
	CONTROL_LOCKOUT_NEGATIVE,
	CONTROL_CAPTURE_POSITIVE,
	CONTROL_CAPTURE_NEGATIVE,
	CONTROL_NEUTRALIZE_POSITIVE,
	CONTROL_NEUTRALIZE_NEGATIVE,
	CONTROL_LOCKOUT_BROKEN_POSITIVE,
	CONTROL_LOCKOUT_BROKEN_NEGATIVE,
	CONTROL_NEW_RATINGS_LEADER_POSITIVE,
	CONTROL_NEW_RATINGS_LEADER_NEGATIVE,
	CONTROL_CAPTURE_BONUS_POSITIVE,
	CONTROL_CAPTURE_BONUS_NEGATIVE,
	CONTROL_CAPTURING_ZONE_POSITIVE,
	CONTROL_CAPTURING_ZONE_NEGATIVE,
	CONTROL_NEUTRALIZING_ZONE_POSITIVE,
	CONTROL_NEUTRALIZING_ZONE_NEGATIVE,
	CONTROL_CONTESTING_ZONE_POSITIVE,
	CONTROL_CONTESTING_ZONE_NEGATIVE,

	LEAD_CHANGE_POSITIVE,
	LEAD_CHANGE_NEGATIVE,
	CLOSE_TO_WINNING_MATCH_POSITIVE,
	CLOSE_TO_WINNING_MATCH_NEGATIVE,
	MATCH_HALFWAY_SCORE_REACHED_POSITIVE,
	MATCH_HALFWAY_SCORE_REACHED_NEGATIVE,
	WIN_BY_LARGE_MARGIN_POSITIVE,
	WIN_BY_LARGE_MARGIN_NEGATIVE,
	WIN_BY_MEDIUM_MARGIN_POSITIVE,
	WIN_BY_MEDIUM_MARGIN_NEGATIVE,
	WIN_BY_SMALL_MARGIN_POSITIVE,
	WIN_BY_SMALL_MARGIN_NEGATIVE,

	KILL_STREAK_CONSECUTIVE_3_POSITIVE,
	KILL_STREAK_CONSECUTIVE_3_NEGATIVE,
	KILL_STREAK_CONSECUTIVE_5_POSITIVE,
	KILL_STREAK_CONSECUTIVE_5_NEGATIVE,
	KILL_STREAK_CONSECUTIVE_10_POSITIVE,
	KILL_STREAK_CONSECUTIVE_10_NEGATIVE,
	KILL_STREAK_CONSECUTIVE_15_POSITIVE,
	KILL_STREAK_CONSECUTIVE_15_NEGATIVE,
    KILL_STREAK_FREQUENT_1_POSITIVE,
	KILL_STREAK_FREQUENT_1_NEGATIVE,
	KILL_STREAK_FREQUENT_2_POSITIVE,
	KILL_STREAK_FREQUENT_2_NEGATIVE,
	KILL_STREAK_FREQUENT_3_POSITIVE,
	KILL_STREAK_FREQUENT_3_NEGATIVE,
	KILL_STREAK_FREQUENT_5_POSITIVE,
	KILL_STREAK_FREQUENT_5_NEGATIVE,


	TREASURE_HUNT_CAPTURING_ZONE_POSITIVE,
	TREASURE_HUNT_CAPTURING_ZONE_NEGATIVE,
	TREASURE_HUNT_CAPTURED_ZONE_POSITIVE,
	TREASURE_HUNT_CAPTURED_ZONE_NEGATIVE,
	TREASURE_HUNT_CONTESTING_ZONE_POSITIVE,
	TREASURE_HUNT_CONTESTING_ZONE_NEGATIVE,


	NESSIE_FIREWORKS_EE,

	_count
}

global enum eCrowdSound
{
	IDLE,
	ANTICIPATION,
	CHANT,
	CAPTURE_START,
	ROUND_POINT,
	PLAYER_DEATH,

	_count
}

global enum eCrowdEvent
{
	GAME_START,
	CHEER,
	VICTORY,
	LOSS,

	_count
}


enum eCrowdNoiseUpdateReason
{
	DECAY,
	SERVER_UPDATE,

	_count
}


global const string SIGNAL_CROWD_NOISE_METER_DECAY_STOP = "CrowdNoiseMeter_Decay_Stop"

const float SCORE_CHECK_INTERVAL											= 0.25

struct
{
	int nextDecayUnixTimestamp												= 0
	int decayIntervalSeconds												= 0
	float decayValue														= 0.0

	float deltaDecayPerSec													= 0.0

	float cheerDeltaTrigger													= 0.0

	float[eCrowdNoiseMeterModifiers._count] crowdNoiseMeterModifiers
















	void functionref() onFirstSpawnCallback

	float localNoiseMeterValue												= 0.0
	float localNoiseMeterDeltaValue											= 0.0

	float localNoiseMeterLastUpdateTime										= 0.0

	string[eCrowdSound._count] crowdSounds									= [ "tdome_crowd_idle_lp"
																				, "tdome_crowd_anticipation_lp"
																				, "tdome_crowd_chant"
																				, "tdome_crowd_capturing_anticipation_lp"
																				, "tdome_crowd_cheer"
																				, "tdome_crowd_death_reaction" ]

	var[eCrowdSound._count] crowdSoundHandles								= [ null
																				, null
																				, null
																				, null
																				, null
																				, null ]

	string[eCrowdEvent._count] crowdEventSounds								= [ "tdome_crowd_cheer_gamestart"
																				, "tdome_crowd_cheer"
																				, "tdome_crowd_cheer_gameend_winner"
																				, "tdome_crowd_cheer_gameend_loose" ]

	table < int, int > crowdSoundOdds

#if DEV
		var meterRui														= null
		bool isServerBroadcasting
#endif

	bool isFirstTimeSpawned													= false


#if DEV
	bool isDecayEnabled														= true
#endif

} file


void function CrowdNoiseMeter_Init()
{
	if ( !CrowdNoiseMeterEnabled() )
		return







#if DEV
		file.isServerBroadcasting = GetCurrentPlaylistVarBool( "crowd_noise_meter_enable_server_broadcasting", false )
#endif

	file.decayIntervalSeconds = GetCurrentPlaylistVarInt( "crowd_noise_meter_decay_interval_seconds", 0 )
	file.decayValue = GetCurrentPlaylistVarFloat( "crowd_noise_meter_decay_value", 0.0 )

	file.deltaDecayPerSec = GetCurrentPlaylistVarFloat( "crowd_noise_meter_delta_decay_rate", 0.5 )

	file.cheerDeltaTrigger = GetCurrentPlaylistVarFloat( "crowd_noise_meter_cheer_delta_trigger", 0.2 )

	
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_HALFWAY_POINT_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_halfway_point_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_HALFWAY_POINT_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_halfway_point_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_CAPTURE_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_capture_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_CAPTURE_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_capture_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_HUGE_LEAD_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_huge_lead_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_HUGE_LEAD_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_huge_lead_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_IMMINENT_WIN_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_imminent_win_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_IMMINENT_WIN_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_imminent_win_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_LOCKOUT_BROKEN_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_lockout_broken_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_LOCKOUT_BROKEN_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_lockout_broken_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_LOCKOUT_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_lockout_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_LOCKOUT_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_lockout_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_NEUTRALIZE_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_neutralize_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_NEUTRALIZE_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_neutralize_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_NEW_RATINGS_LEADER_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_new_ratings_leader_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_NEW_RATINGS_LEADER_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_new_ratings_leader_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_CAPTURE_BONUS_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_capture_bonus_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_CAPTURE_BONUS_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_capture_bonus_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_CAPTURING_ZONE_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_capturing_zone_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_CAPTURING_ZONE_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_capturing_zone_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_NEUTRALIZING_ZONE_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_neutralizing_zone_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_NEUTRALIZING_ZONE_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_neutralizing_zone_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_CONTESTING_ZONE_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_contesting_zone_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CONTROL_CONTESTING_ZONE_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_control_contesting_zone_negative", 0.0 )


	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.LEAD_CHANGE_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_lead_change_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.LEAD_CHANGE_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_lead_change_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CLOSE_TO_WINNING_MATCH_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_close_to_winning_match_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.CLOSE_TO_WINNING_MATCH_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_close_to_winning_match_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.MATCH_HALFWAY_SCORE_REACHED_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_match_halfway_score_reached_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.MATCH_HALFWAY_SCORE_REACHED_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_match_halfway_score_reached_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.WIN_BY_LARGE_MARGIN_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_win_by_large_margin_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.WIN_BY_LARGE_MARGIN_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_win_by_large_margin_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.WIN_BY_MEDIUM_MARGIN_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_win_by_medium_margin_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.WIN_BY_MEDIUM_MARGIN_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_win_by_medium_margin_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.WIN_BY_SMALL_MARGIN_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_win_by_small_margin_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.WIN_BY_SMALL_MARGIN_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_win_by_small_margin_negative", 0.0 )


	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_CONSECUTIVE_3_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_consecutive_3_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_CONSECUTIVE_3_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_consecutive_3_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_CONSECUTIVE_5_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_consecutive_5_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_CONSECUTIVE_5_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_consecutive_5_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_CONSECUTIVE_10_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_consecutive_10_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_CONSECUTIVE_10_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_consecutive_10_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_CONSECUTIVE_15_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_consecutive_15_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_CONSECUTIVE_15_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_consecutive_15_negative", 0.0 )

    file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_FREQUENT_1_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_frequent_1_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_FREQUENT_1_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_frequent_1_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_FREQUENT_2_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_frequent_2_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_FREQUENT_2_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_frequent_2_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_FREQUENT_3_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_frequent_3_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_FREQUENT_3_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_frequent_3_negative", 0.0 )

	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_FREQUENT_5_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_frequent_5_positive", 0.0 )
	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.KILL_STREAK_FREQUENT_5_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_kill_streak_frequent_5_negative", 0.0 )


		file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.TREASURE_HUNT_CAPTURING_ZONE_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_treasure_hunt_capturing_zone_positive", 0.0 )
		file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.TREASURE_HUNT_CAPTURING_ZONE_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_treasure_hunt_capturing_zone_negative", 0.0 )

		file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.TREASURE_HUNT_CAPTURED_ZONE_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_treasure_hunt_captured_zone_positive", 0.0 )
		file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.TREASURE_HUNT_CAPTURED_ZONE_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_treasure_hunt_captured_zone_negative", 0.0 )

		file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.TREASURE_HUNT_CONTESTING_ZONE_POSITIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_treasure_hunt_contesting_zone_positive", 0.0 )
		file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.TREASURE_HUNT_CONTESTING_ZONE_NEGATIVE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_modifier_treasure_hunt_contesting_zone_negative", 0.0 )


	file.crowdNoiseMeterModifiers[eCrowdNoiseMeterModifiers.NESSIE_FIREWORKS_EE] = GetCurrentPlaylistVarFloat( "crowd_noise_meter_nessie_fireworks_ee_positive", 1 )
	

	RegisterSignal( SIGNAL_CROWD_NOISE_METER_DECAY_STOP )









	AddCallback_GameStateEnter( eGameState.WaitingForPlayers, CrowdNoiseMeter_OnGameWaitingForPlayers )
	AddCallback_GameStateEnter( eGameState.Playing, CrowdNoiseMeter_OnGamePlaying )


		AddCallback_LocalClientPlayerSpawned( CrowdNoiseMeter_OnPlayerSpawned )
		AddOnDeathCallback( "player", CrowdNoiseMeter_OnPlayerKilled )


		
		file.crowdSoundOdds[eCrowdSound.CAPTURE_START] <- 100


		
		int roundPointTriggerOdds = GetCurrentPlaylistVarInt( "crowd_noise_meter_round_point_trigger_odds", 0 )
		if ( roundPointTriggerOdds > 0 )
			file.crowdSoundOdds[eCrowdSound.ROUND_POINT] <- roundPointTriggerOdds

		int playerDeathTriggerOdds = GetCurrentPlaylistVarInt( "crowd_noise_meter_player_death_trigger_odds", 0 )
		if ( playerDeathTriggerOdds > 0 )
			file.crowdSoundOdds[eCrowdSound.PLAYER_DEATH] <- playerDeathTriggerOdds

}

bool function CrowdNoiseMeterEnabled()
{
	return GetCurrentPlaylistVarBool( "crowd_noise_meter_enabled", false )
}

void function CrowdNoiseMeter_RegisterNetworking()
{
	Remote_RegisterClientFunction( "ServerCallback_CrowdNoiseMeterValueUpdate", "float", -1.0, 1.0, 32, "int", 0, INT_MAX ) 






}

void function CrowdNoiseMeter_OnGameWaitingForPlayers()
{

		file.isFirstTimeSpawned = true

}

void function CrowdNoiseMeter_OnGamePlaying()
{



		thread CLIENT_CrowdNoiseMeter_Think()
		thread CLIENT_ScoreCheck_Thread()

}


void function CrowdNoiseMeter_OnPlayerSpawned( entity localPlayer )
{
	thread OnFirstTimeSpawned_Thread( localPlayer )
}



void function CrowdNoiseMeter_OnPlayerKilled( entity victim )
{
	entity player = GetLocalClientPlayer()
	if ( !IsValid( player ) || (player != victim) )
		return

	ToggleCrowdSoundOnEntity( player, eCrowdSound.PLAYER_DEATH, true )
}



void function OnFirstTimeSpawned_Thread( entity localPlayer )
{
	if ( !IsValid( localPlayer ) )
		return

	thread CLIENT_CrowdNoiseMeter_Think() 

	if ( !file.isFirstTimeSpawned )
		return

	file.isFirstTimeSpawned = false

	if ( GetGameState() < eGameState.Playing )
	{
		ClWaittillGameStateOrHigher( eGameState.Playing )
	}

	if ( file.onFirstSpawnCallback != null )
	{
		file.onFirstSpawnCallback()
	}
}



void function SetCallback_CrowdNoiseMeter_OnFirstSpawn( void functionref() func )
{
	file.onFirstSpawnCallback = func
}



void function ServerCallback_CrowdNoiseMeterValueUpdate( float updatedCrowdNoiseMeterValue, int nextDecayIntervalUnixTimestamp )
{
	if ( !CrowdNoiseMeterEnabled() )
		return

	
	

	CLIENT_SetCrowdNoiseMeterValue( updatedCrowdNoiseMeterValue, eCrowdNoiseUpdateReason.SERVER_UPDATE )

	if ( GamePlayingOrSuddenDeath() )
	{
		thread CLIENT_CrowdNoiseMeter_Think()
	}
}































void function CLIENT_SetCrowdNoiseMeterValue( float updatedCrowdNoiseMeterValue, int updateReason )
{
	
	float crowdNoiseMeterDelta = updatedCrowdNoiseMeterValue - file.localNoiseMeterValue
	file.localNoiseMeterValue = clamp( updatedCrowdNoiseMeterValue, 0.0, 1.0 )
	

	
	
	float curtime = Time()
	float deltaDecay = min( fabs( file.localNoiseMeterDeltaValue ), (curtime - file.localNoiseMeterLastUpdateTime) * file.deltaDecayPerSec )
	if ( file.localNoiseMeterDeltaValue > 0 )
		deltaDecay *= -1

	file.localNoiseMeterDeltaValue += deltaDecay + crowdNoiseMeterDelta
	file.localNoiseMeterLastUpdateTime = curtime
	

	Sound_SetCrowdState( file.localNoiseMeterValue * 100, file.localNoiseMeterDeltaValue * 100 )

#if DEV
		if ( IsValid( file.meterRui ) )
		{
			RuiSetFloat( file.meterRui, "progress", file.localNoiseMeterValue )
		}
#endif

	if ( !GamePlayingOrSuddenDeath() )
		return

	entity player = GetLocalClientPlayer()
	if ( !IsValid( player ) )
		return

	
	if ( updateReason == eCrowdNoiseUpdateReason.DECAY )
		return

	if ( fabs( file.localNoiseMeterDeltaValue ) >= file.cheerDeltaTrigger )
	{
		TriggerCrowdEventOnEntity( player, eCrowdEvent.CHEER )
	}
}











































































































































































































































































































































































void function CLIENT_CrowdNoiseMeter_Think()
{
	Assert( IsNewThread(), "Must be threaded off" )

	entity player = GetLocalClientPlayer()
	if ( !IsValid( player ) )
		return

#if DEV
		if ( !file.isServerBroadcasting )
			return
#endif

	Signal( player, SIGNAL_CROWD_NOISE_METER_DECAY_STOP )
	EndSignal( player, SIGNAL_CROWD_NOISE_METER_DECAY_STOP )
	EndSignal( player, "OnDeath", "OnDestroy")

	if ( (file.crowdSoundHandles[eCrowdSound.ANTICIPATION] == null) || !IsSoundStillPlaying( file.crowdSoundHandles[eCrowdSound.ANTICIPATION] ) )
	{
		file.crowdSoundHandles[eCrowdSound.ANTICIPATION] = EmitSoundOnEntity_NoTimeScale( player, file.crowdSounds[eCrowdSound.ANTICIPATION] )


			SetPlayThroughKillReplay( file.crowdSoundHandles[eCrowdSound.ANTICIPATION] )
			SetPlayThroughPOVTransitions( file.crowdSoundHandles[eCrowdSound.ANTICIPATION] )

	}

	if ( file.decayIntervalSeconds == 0 )
		return

	while ( CrowdNoiseMeterEnabled() )
	{
		file.nextDecayUnixTimestamp = GetUnixTimestamp() + file.decayIntervalSeconds
		wait file.decayIntervalSeconds

#if DEV
			if ( !file.isDecayEnabled )
				continue
#endif

		UpdateLocalCrowdNoiseMeter( file.decayValue, eCrowdNoiseUpdateReason.DECAY )
	}
}



void function CLIENT_ScoreCheck_Thread()
{
	Assert( IsNewThread(), "Must be threaded off" )

	entity player = GetLocalClientPlayer()
	if ( !IsValid( player ) || player.IsBot() )
		return

#if DEV
		if ( !file.isServerBroadcasting )
			return
#endif

	int prevScore = 0
	while ( GetGameState() == eGameState.Playing )
	{
		wait SCORE_CHECK_INTERVAL

		if ( !IsValid( player ) )
			continue

		int currentScore = 0
		if ( AllianceProximity_IsUsingAlliances() )
		{
			int myAlliance = AllianceProximity_GetAllianceFromTeamWithObserverCorrection( player.GetTeam() )
			currentScore = GetAllianceTeamsScore( myAlliance )
		}
		else
		{
			currentScore = GameRules_GetTeamScore( player.GetTeam() )
		}

		if ( currentScore == prevScore )
			continue

		int maxScore = GetScoreLimit_FromPlaylist()
		int pointsToMaxScore = maxScore - currentScore
		int prevPointsToMaxScore = maxScore - prevScore

		if ( pointsToMaxScore == 1 )
		{
			ToggleCrowdSoundOnEntity( player, eCrowdSound.ROUND_POINT, true )
		}
		else if ( prevPointsToMaxScore == 1 )
		{
			ToggleCrowdSoundOnEntity( player, eCrowdSound.ROUND_POINT, false )
		}

		prevScore = currentScore
	}
}



void function UpdateLocalCrowdNoiseMeter( float crowdNoiseMeterDelta, int updateReason )
{
	float updatedCrowdNoiseMeterValue = file.localNoiseMeterValue + crowdNoiseMeterDelta
	CLIENT_SetCrowdNoiseMeterValue( updatedCrowdNoiseMeterValue, updateReason )
}



void function TriggerCrowdEventOnEntity( entity ent, int crowdEvent )
{
	if ( !CrowdNoiseMeterEnabled() )
		return

	Assert( (crowdEvent >= 0) && (crowdEvent < eCrowdEvent._count), "Attempting to trigger an invalid crowd event [ " + crowdEvent + " ]" )
	Assert( IsValid( ent ), "Attempting to trigger crowd event on invalid entity" )

	var soundHandle = EmitSoundOnEntity_NoTimeScale( ent, file.crowdEventSounds[crowdEvent] )


		SetPlayThroughKillReplay( soundHandle )
		SetPlayThroughPOVTransitions( soundHandle )

}



void function ToggleCrowdSoundOnEntity( entity ent, int crowdSound, bool isEnabled )
{
	if ( !CrowdNoiseMeterEnabled() )
		return

	Assert( (crowdSound >= 0) && (crowdSound < eCrowdSound._count), "Attempting to toggle an invalid crowd sound [ " + crowdSound + " ]" )
	Assert( IsValid( ent ), "Attempting to trigger crowd sound on invalid entity" )

	if ( !(crowdSound in file.crowdSoundOdds) )
		return

	if ( isEnabled )
	{
		int percentToTrigger = file.crowdSoundOdds[crowdSound]
		int randInt = RandomInt( 100 )
		if ( randInt >= percentToTrigger )
			return

		if ( (file.crowdSoundHandles[crowdSound] == null) || !IsSoundStillPlaying( file.crowdSoundHandles[crowdSound] ) )
		{
			file.crowdSoundHandles[crowdSound] = EmitSoundOnEntity_NoTimeScale( ent, file.crowdSounds[crowdSound] )


				SetPlayThroughKillReplay( file.crowdSoundHandles[crowdSound] )
				SetPlayThroughPOVTransitions( file.crowdSoundHandles[crowdSound] )

		}
	}
	else
	{
		if ( file.crowdSoundHandles[crowdSound] != null )
		{
			StopSoundOnEntity( ent, file.crowdSounds[crowdSound] )
			file.crowdSoundHandles[crowdSound] = null
		}
	}
}



void function CrowdNoiseMeter_PlayGameEndSound( entity player, bool isOnWinningTeam )
{
	if ( !CrowdNoiseMeterEnabled() )
		return

	if ( !IsValid( player ) )
		return

	string endSound = isOnWinningTeam ? file.crowdEventSounds[eCrowdEvent.VICTORY] : file.crowdEventSounds[eCrowdEvent.LOSS]
	var endSoundHandle = EmitSoundOnEntity_NoTimeScale( player, endSound )


		SetPlayThroughKillReplay( endSoundHandle )
		SetPlayThroughPOVTransitions( endSoundHandle )

}


#if DEV
void function DEV_Cl_ToggleCrowdNoiseMeterOnPlayer()
{
	entity player = GetLocalClientPlayer()
	if ( !IsValid( player ) )
		return

	Signal( player, SIGNAL_CROWD_NOISE_METER_DECAY_STOP )

	foreach ( var soundHandle in file.crowdSoundHandles )
	{
		if ( soundHandle != null && IsSoundStillPlaying( soundHandle ) )
		{
			StopSound( soundHandle )
		}
	}

	Remote_ServerCallFunction( "ClientCallback_DevToggleCrowdNoiseMeterOnPlayer" )
}
#endif

#if DEV
void function DEV_Cl_ToggleCrowdNoiseMeterDebug()
{
	if ( IsValid( file.meterRui ) )
	{
		RuiDestroyIfAlive( file.meterRui )
		file.meterRui = null
	}
	else
	{
		file.meterRui = CreateFullscreenRui( $"ui/debuff_progress.rpak", HUD_Z_BASE )
		RuiSetFloat( file.meterRui, "progress", file.localNoiseMeterValue )
	}
}
#endif

#if DEV
void function DEV_Cl_ToggleCrowdNoiseMeterOnServer()
{
	Remote_ServerCallFunction( "ClientCallback_DevToggleCrowdNoiseMeterOnServer" )
}
#endif



































