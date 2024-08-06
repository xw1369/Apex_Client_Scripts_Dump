global function GetPlaylistVar_RespawnTokenEnabled


global function Sh_Respawn_Token_Init
global function Sh_Respawn_Token_IsPlayerRespawnDisabled








global function Respawn_Token_CanLocalPlayerRespawn
global function Respawn_Token_CanLocalPlayerRespawnOrIsRespawning
global function Respawn_Token_SetCanLocalPlayerRespawn



global function Respawn_Token_Announcement_RespawnsDisabled
global function Respawn_Token_OnReconnect_UpdateRUI








const string NETFUNC_ANNOUNCEMENT_RESPAWNS_DISABLED = "Respawn_Token_Announcement_RespawnsDisabled"
const string NETFUNC_ON_RECONNECT_UPDATE_RUI = "Respawn_Token_OnReconnect_UpdateRUI"

const string NETVAR_SQUAD_RESPAWN_TOKENS = "Squad_Respawn_Tokens"
const string NETVAR_RESPAWN_DISABLED = "Squad_Respawn_Disabled"

const float TIME_ANNOUNCEMENT_RESPAWN_DISABLED = 7.0
const float TIME_ANNOUNCEMENT_SQUAD_RESPAWN_TOKENS_REMAINING = 7.0

const int PRIORITY_ANNOUNCEMENT_SQUAD_RESPAWN_TOKENS_REMAINING = 1000







const asset RUI_RESPAWN_INFO = $"ui/gamestate_info_survival_solos.rpak"
const asset RUI_RESPAWN_HUD = $"ui/gamestate_survival_respawn_token.rpak"

const string RUIVAR_RESPAWN_DISABLED = "respawnsDisabled"

const string SFX_SQUAD_RESPAWN_TOKENS_REMAINING_TWO = "UI_3Strikes_Widget_Stinger_Strike1"
const string SFX_SQUAD_RESPAWN_TOKENS_REMAINING_ONE = "UI_3Strikes_Widget_Stinger_Strike1"
const string SFX_SQUAD_RESPAWN_TOKENS_REMAINING_ZERO = "UI_3Strikes_Widget_Stinger_Strike1"
const string SFX_RESPAWN_DISABLED = "UI_3Strikes_RespawningDisabled"


global enum eRespawnTokenType
{
	INVALID = -1,
	INDIVIDUAL,  
	SQUAD,       
	BANK,        
	_COUNT
}

struct
{





	int  squadRespawnTokens = 0
	int  squadRespawnTokensViewPlayer = 0
	int  lastSquadRespawnTokenAnnouncement = -1
	bool canRespawn = false
	bool isRespawning = false



	var nestedRespawnTokenRui
	bool squadEliminated = false

} file



void function Sh_Respawn_Token_Init()
{
	if ( !GetPlaylistVar_RespawnTokenEnabled() )
		return

	Sh_Respawn_Token_RegisterNetworking()

















	RegisterNetVarIntChangeCallback( NETVAR_SQUAD_RESPAWN_TOKENS, Respawn_Token_SquadRespawnTokensChanged )

	Announcements_SetOnSetupAnnouncement_RemainingRespawns( Respawn_Token_OnSetupAnnouncement_RemainingRespawns )
	AddCallback_PlayerFreefallActiveChanged( Respawn_Token_PlayerFreefallActiveChanged )
	DeathScreen_SetIsPlayerWaitingForRespawnFunc( Respawn_Token_IsPlayerWaitingForRespawn )

	Respawn_Token_OverrideGameState()


	Assert( GetPlaylistVar_RespawnTokenType() != eRespawnTokenType.INVALID, "Playlist variable \"respawn_token_type\" must be set when using Respawn Tokens with \"respawn_token_enabled\"!" )
}



void function Sh_Respawn_Token_RegisterNetworking()
{
	RegisterNetworkedVariable( NETVAR_SQUAD_RESPAWN_TOKENS, SNDC_PLAYER_EXCLUSIVE, SNVT_INT, 0.0 )
	RegisterNetworkedVariable( NETVAR_RESPAWN_DISABLED, SNDC_PLAYER_EXCLUSIVE, SNVT_BOOL, false )

	Remote_RegisterClientFunction( NETFUNC_ANNOUNCEMENT_RESPAWNS_DISABLED )
	Remote_RegisterClientFunction( NETFUNC_ON_RECONNECT_UPDATE_RUI )
}



void function Respawn_Token_OverrideGameState()
{
	ClGameState_RegisterGameStateAsset( RUI_RESPAWN_INFO )
}



bool function Sh_Respawn_Token_IsPlayerRespawnDisabled( entity player )
{
	return player.GetPlayerNetBool( NETVAR_RESPAWN_DISABLED )
}







































































































































































































































bool function Respawn_Token_CanLocalPlayerRespawn()
{
	return file.canRespawn
}



bool function Respawn_Token_CanLocalPlayerRespawnOrIsRespawning()
{
	return file.canRespawn || file.isRespawning
}



void function Respawn_Token_SetCanLocalPlayerRespawn( bool isRespawning, bool canRespawn )
{
	file.isRespawning = isRespawning
	file.canRespawn = canRespawn
}















int function _Respawn_Token_GetPlayerRespawnTokens( entity player )
{
	if ( !IsValid( player ) )
	{
		return 0
	}

	return player.GetPlayerNetInt( NETVAR_SQUAD_RESPAWN_TOKENS )
}





































































































var function Respawn_Token_OnSetupAnnouncement_RemainingRespawns( AnnouncementData announcement )
{
	entity player = GetLocalClientPlayer()
	if ( !IsValid( player ) )
		return null

	var rui = RuiCreate( $"ui/announcement_solos_respawns_remaining.rpak", clGlobal.topoFullScreen, RUI_DRAW_POSTEFFECTS, RUI_SORT_SCREENFADE + 1 )
	RuiSetInt( rui, "squadLives", file.squadRespawnTokens )
	RuiSetInt( rui, "strikeTotal", RespawnTokensToStrikes( file.squadRespawnTokens ) )

	EmitSoundOnEntity( player, announcement.soundAlias )
	return rui
}



void function Respawn_Token_SquadRespawnTokensChanged( entity player, int newValue )
{
	if ( !IsValid( player ) )
		return

	if ( player == GetLocalViewPlayer() )
	{
		file.squadRespawnTokensViewPlayer = newValue
	}

	if ( player == GetLocalClientPlayer() )
	{
		file.squadRespawnTokens = newValue

		
		if ( file.squadRespawnTokens <= -1 )
		{
			file.squadEliminated = true
		}

		if ( file.lastSquadRespawnTokenAnnouncement == -1 )
		{
			file.lastSquadRespawnTokenAnnouncement = newValue
		}
	}

	UpdateRui_SquadRespawnTokens()
}



void function Respawn_Token_PlayerFreefallActiveChanged( entity player, bool isFreefallActive )
{
	if ( !isFreefallActive )
		return

	if ( file.lastSquadRespawnTokenAnnouncement == file.squadRespawnTokens )
		return

	Announcement_SquadRespawnTokensRemaining()
	file.lastSquadRespawnTokenAnnouncement = file.squadRespawnTokens
}



void function UpdateRui_SquadRespawnTokens()
{
	var gamestateRui = ClGameState_GetRui()
	if ( gamestateRui != null && RuiIsAlive( gamestateRui ) && file.nestedRespawnTokenRui == null || !RuiIsAlive( file.nestedRespawnTokenRui ) )
	{
		RuiDestroyNestedIfAlive( gamestateRui, "respawnTokenHudHandle" )
		file.nestedRespawnTokenRui = RuiCreateNested( gamestateRui, "respawnTokenHudHandle", RUI_RESPAWN_HUD )
	}

	if ( file.nestedRespawnTokenRui != null && RuiIsAlive( file.nestedRespawnTokenRui ) )
	{
		RuiSetInt( file.nestedRespawnTokenRui, "squadLives", maxint( file.squadRespawnTokens, 0 ) )
		RuiSetInt( file.nestedRespawnTokenRui, "viewPlayerLives", maxint( file.squadRespawnTokensViewPlayer, 0 ) )
	}

	if ( IsValid( GetLocalClientPlayer() ) )
	{
		bool isRespawning = file.squadRespawnTokens >= 0 && !file.squadEliminated
		bool canRespawn = !file.squadEliminated && ( ( file.squadRespawnTokens > 0 && IsAlive( GetLocalClientPlayer() ) ) || ( file.squadRespawnTokens >= 0 && !IsAlive( GetLocalClientPlayer() ) ) )

		Respawn_Token_SetCanLocalPlayerRespawn( isRespawning, canRespawn )
		RunUIScript( "Respawn_Token_SetCanLocalPlayerRespawn", isRespawning, canRespawn )
	}
}



void function UpdateRui_RespawnState( bool newState )
{
	var rui = ClGameState_GetRui()
	if ( rui != null )
	{
		RuiSetBool( rui, RUIVAR_RESPAWN_DISABLED, newState )
	}

	Respawn_Token_SetCanLocalPlayerRespawn( newState, newState )
	RunUIScript( "Respawn_Token_SetCanLocalPlayerRespawn", newState, newState )
}



void function Respawn_Token_OnReconnect_UpdateRUI()
{
	UpdateRui_SquadRespawnTokens()
}



void function Announcement_SquadRespawnTokensRemaining()
{
	string message = Localize( "#SURVIVAL_MODE_SOLOS_RESPAWN_TOKEN_USED" )
	string announcementSFX

	if ( file.squadRespawnTokens >= 2 )
	{
		announcementSFX = SFX_SQUAD_RESPAWN_TOKENS_REMAINING_TWO
	}
	else if ( file.squadRespawnTokens >= 1 )
	{
		announcementSFX = SFX_SQUAD_RESPAWN_TOKENS_REMAINING_ONE
	}
	else
	{
		announcementSFX = SFX_SQUAD_RESPAWN_TOKENS_REMAINING_ZERO
	}

	AnnouncementData announcement = Announcement_Create( message )
	announcement.priority = 202 

	Announcement_SetDuration( announcement, TIME_ANNOUNCEMENT_SQUAD_RESPAWN_TOKENS_REMAINING )
	Announcement_SetStyle( announcement, ANNOUNCEMENT_STYLE_REMAINING_RESPAWNS )
	Announcement_SetPriority( announcement, PRIORITY_ANNOUNCEMENT_SQUAD_RESPAWN_TOKENS_REMAINING )

	AnnouncementFromClass( GetLocalViewPlayer(), announcement )
	EmitSoundOnEntity( GetLocalViewPlayer(), announcementSFX )

	UpdateRui_SquadRespawnTokens()
}




void function Respawn_Token_Announcement_RespawnsDisabled()
{
	int currentDeathFieldStage = SURVIVAL_GetCurrentDeathFieldStage()
	if ( currentDeathFieldStage >= 0 )
	{
		AnnouncementData announcement = Announcement_Create( Localize( "#SURVIVAL_CIRCLE_STARTING" ) )

		Announcement_SetSubText( announcement, GetAnnouncementSubtextString( currentDeathFieldStage + 1 ) )
		Announcement_SetHeaderText( announcement, "" ) 
		Announcement_SetOptionalTextArgsArray( announcement, [ "true" ] )
		Announcement_SetAdditionalText( announcement, "#SURVIVAL_MODE_SOLOS_RESPAWNS_DISABLED" )
		
		Announcement_SetDuration( announcement, TIME_ANNOUNCEMENT_RESPAWN_DISABLED )
		Announcement_SetStyle( announcement, ANNOUNCEMENT_STYLE_CIRCLE_WARNING )
		Announcement_SetSoundAlias( announcement, SFX_RESPAWN_DISABLED )
		Announcement_SetPriority( announcement, 209 )
		Announcement_SetPurge( announcement, false ) 

		AnnouncementFromClass( GetLocalViewPlayer(), announcement )
	}

	UpdateRui_RespawnState( false )
	UpdateRui_SquadRespawnTokens()
}



int function Respawn_Token_GetMaxSquadRespawnTokens()
{
	return GetStartingRespawnCount() + 1
}



bool function Respawn_Token_IsPlayerWaitingForRespawn( entity player )
{
	if ( !IsValid( player ) || ( player != GetLocalClientPlayer() ) )
		return false

	if ( Sh_Respawn_Token_IsPlayerRespawnDisabled( player ) )
		return false

	if ( file.squadEliminated )
		return false

	
	return ( file.squadRespawnTokens >= 0 )
}



















int function RespawnTokensToStrikes( int respawnTokens )
{
	
	const int MAX_STRIKES = 3

	int strikes = minint( MAX_STRIKES - respawnTokens - 1, 3 )
	return maxint( strikes, 0 )
}

bool function GetPlaylistVar_RespawnTokenEnabled()
{
	return GetCurrentPlaylistVarBool( "respawn_token_enabled", false )
}

int function GetPlaylistVar_RespawnTokenType()
{
	string playlistTokenType = GetCurrentPlaylistVarString( "respawn_token_type", "" ).tolower()

	int tokenType = eRespawnTokenType.INVALID
	bool typeFound = false
	for( int i = 0; i < eRespawnTokenType._COUNT; i++ )
	{
		string enumType = GetEnumString( "eRespawnTokenType", i ).tolower()
		if ( enumType.tolower() == playlistTokenType )
		{
			tokenType = i
			typeFound = true
			break
		}
	}

	Assert( typeFound, "Playlist Respawn Token type '" + playlistTokenType + "' is not a specified enumerator." )

	return tokenType
}

bool function GetPlaylistVar_AmmoOnKillEnabled()
{
	return GetCurrentPlaylistVarBool( "respawn_token_ammo_on_kill_enabled", false )
}

float function GetPlaylistVar_AmmoOnKillMultiplier()
{
	return GetCurrentPlaylistVarFloat( "respawn_token_ammo_on_kill_multiplier", 1.0 )
}

int function GetPlaylistVar_AmmoOnKillBoxesToSpawn()
{
	return GetCurrentPlaylistVarInt( "respawn_token_ammo_on_kill_spawns", 4 )
}

bool function GetPlaylistVar_ArmorOnKillEnabled()
{
	return GetCurrentPlaylistVarBool( "respawn_token_armor_on_kill_enabled", false )
}

bool function GetPlaylistVar_PingDeathLocationEnabled()
{
	return GetCurrentPlaylistVarBool( "respawn_token_ping_death_location", false )
}

int function GetPlaylistVar_RespawnDisabledDeathstage()
{
	return GetCurrentPlaylistVarInt( "respawn_token_respawn_disable_deathstage_close", 4 )
}

int function GetPlaylistVar_XPForRespawnToken()
{
	return GetCurrentPlaylistVarInt( "respawn_token_xp_for_respawn_token", 500 )
}

bool function GetPlaylistVar_TakeTokenOnDisconnect()
{
	return GetCurrentPlaylistVarBool( "respawn_token_take_token_on_disconnect", false )
}

bool function GetPlaylistVar_KillPlayerOnDisconnect()
{
	return GetCurrentPlaylistVarBool( "respawn_token_kill_player_on_disconnect", false )
}
