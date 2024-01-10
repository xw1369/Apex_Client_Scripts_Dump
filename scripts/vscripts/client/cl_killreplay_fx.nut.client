global function ClientCodeCallback_GameTimescaleChanged
global function ServerCallback_PreKillReplaySounds
global function ClKillReplayFX_Init

struct {
	var killreplayVictimTagRUI = null
	var killReplayBanner = null
	var killReplayBlackBar = null
	var killreplayEndUntilRespawnSound = null

	bool wasInSlowmo = false
	var slowmoSoundHandle = null
	var deathSoundHandle = null
} file

void function ClientCodeCallback_GameTimescaleChanged( float newTimescale )
{
	if ( file.wasInSlowmo && newTimescale >= 1 )
	{
		file.wasInSlowmo = false
	}
	else if ( IsWatchingKillReplay() && !file.wasInSlowmo && newTimescale <= 1 )
	{
		file.slowmoSoundHandle = EmitSoundOnEntity_NoTimeScale( GetLocalClientPlayer(), "KillCam_UI_SlowMo_Stinger" )
		file.wasInSlowmo = true
	}
}

void function ServerCallback_PreKillReplaySounds()
{
	var killreplayBeginSoundHandle = EmitUISound( "KillCam_UI_LeadIn_Stinger" )
	SetPlayThroughKillReplay( killreplayBeginSoundHandle )
	SetPlayThroughPOVTransitions( killreplayBeginSoundHandle )
}

void function ClKillReplayFX_Init()
{
	AddCallback_KillReplayStarted( ClKillReplayFX_ReplayBegin )
	AddCallback_KillReplayEnded( ClKillReplayFX_ReplayEnd )
	AddCallback_GameStateEnter( eGameState.Resolution, ClKillReplayFX_GamestateResolutionCleanup )
	AddCallback_OnPlayerLifeStateChanged( ClKillReplayFX_PlayerRespawned )
	AddLocalPlayerDidDamageCallback( ClKillReplayFX_PlayerDidDamage )
}

void function ClKillReplayFX_ReplayBegin()
{
	Assert( file.killreplayVictimTagRUI == null )
	entity victim = GetLocalClientPlayer().GetKillReplayVictim()

	
	if ( IsValid( victim ) && victim.IsPlayer() )
	{
		string playerName
		if ( victim == GetLocalClientPlayer() )
		{
			playerName = Localize( "#WAYPOINT_YOU" )
		}
		else
		{
			playerName = GetPlayerNameUnlessAnonymized( ToEHI( victim ) )
		}

		file.killreplayVictimTagRUI = RuiCreate( $"ui/killreplay_player_overhead.rpak", clGlobal.topoFullScreen, RUI_DRAW_HUD, RuiCalculateDistanceSortKey( GetLocalViewPlayer().EyePosition(), victim.GetOrigin() ) )
		InitHUDRui( file.killreplayVictimTagRUI )
		RuiTrackFloat3( file.killreplayVictimTagRUI, "pos", victim, RUI_TRACK_POINT_FOLLOW, victim.LookupAttachment( "HEADSHOT" ) )
		RuiSetString( file.killreplayVictimTagRUI, "name", playerName )
	}

	if ( IsReplayRoundWinning() )
	{
		file.killReplayBlackBar = CreateFullscreenRui( $"ui/death_screen_black_bar.rpak", 1000 )
		file.killReplayBanner = CreateFullscreenRui( $"ui/death_screen_final_kill_header.rpak", 5000 )
	}
}

void function ClKillReplayFX_ReplayEnd()
{
	
	if ( file.killreplayVictimTagRUI != null )
	{
		RuiDestroyIfAlive( file.killreplayVictimTagRUI )
		file.killreplayVictimTagRUI = null
	}

	if ( IsReplayRoundWinning() )
	{
		if ( file.killReplayBlackBar != null )
		{
			RuiDestroyIfAlive( file.killReplayBlackBar )
			file.killReplayBlackBar = null
		}

		if ( file.killReplayBanner != null )
		{
			RuiDestroyIfAlive( file.killReplayBanner )
			file.killReplayBanner = null
		}
	}
	else
	{
		file.killreplayEndUntilRespawnSound = EmitUISound( "duck_for_death_killcam_midgame" )
		SetPlayThroughKillReplay( file.killreplayEndUntilRespawnSound )
		SetPlayThroughPOVTransitions( file.killreplayEndUntilRespawnSound )
	}
}

void function ClKillReplayFX_PlayerRespawned( entity player, int oldLifeState, int newLifeState )
{
	if ( player == GetLocalClientPlayer() && newLifeState == LIFE_ALIVE && file.killreplayEndUntilRespawnSound != null )
	{
		StopSound( file.killreplayEndUntilRespawnSound )
	}
}

void function ClKillReplayFX_GamestateResolutionCleanup()
{
	if ( file.deathSoundHandle != null )
	{
		StopSound( file.deathSoundHandle )
	}
}

void function ClKillReplayFX_PlayerDidDamage( entity attacker, entity victim, vector damagePosition, int damageType, float damageAmount )
{
	if ( !IsWatchingKillReplay() )
		return

	if ( IsReplayRoundWinning() && victim == GetLocalClientPlayer().GetKillReplayVictim() )
	{
		if ( bool( damageType & DF_KILLSHOT ) )
		{
			if ( file.slowmoSoundHandle != null )
			{
				StopSound( file.slowmoSoundHandle )
			}

			file.deathSoundHandle = EmitSoundOnEntity_NoTimeScale( GetLocalClientPlayer(), "player_death_killcam" )
			SetPlayThroughKillReplay( file.deathSoundHandle )
			SetPlayThroughPOVTransitions( file.deathSoundHandle )
		}
	}
}
