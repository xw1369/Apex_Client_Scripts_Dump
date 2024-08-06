
global function LootbinResetEnabled
global function LootbinReset_AllowPerkBinReuse_Enabled
global function MythicBin_ExtendedUseSuccess
global function MythicBin_OnUse
global function LootbinReset_UpdatedGroundLoot_Enabled

global function LootbinReset_UseUniqueSpawnSources_Enabled


global function LootBinReset_IsLootBinSpecial
global function LootBinReset_IsLootBin_GoldBin
global function LootBinReset_IsLootBin_Mythic
global function LootBinReset_LootBinsHaveReset

global function LootBinReset_MythicBins_HaveUniqueInteractPrompt
global function ServerToClient_MythicBinOpened







global function Sh_Lootbin_Reset_Init







const string MYTHIC_BIN_USE_SFX = "menu_deny"
global const string MYTHIC_BIN_SKIN_NAME = "Reroll_Mythic"
global const string GOLD_BIN_SKIN_NAME = "Reroll_Gold"
global const string REGULAR_BIN_RESET_SKIN_NAME = "Default_Alt_Emissive"
global const string SUPPORT_BIN_RESET_SKIN_NAME = "Secret_Alt_Emissive"
global const string ASSAULT_BIN_RESET_SKIN_NAME = "Munitions_Alt_Emissive"





















const BIN_RESET_FX = $"P_LBReset_CP"
const vector COLOR_GOLD_BIN_VFX = <255, 190, 70>
const vector COLOR_MYTHIC_BIN_VFX = <232, 53, 86>
const float BIN_RESET_OPENED_BIN_UPDATE_TIME_OFFSET = 1.4

const string MYTHIC_BIN_OPENING_LOOP_SFX = "Lootbin_Mythic_Unlock_Loop"


const string FUNCNAME_LootBinReset_Warning_Announcement = "ServertoClient_LootBinReset_Warning_Announcement"
const string FUNCNAME_LootBinReset_Warning_HUD_Indicator_Set = "ServertoClient_LootBinReset_Warning_HUD_Indicator_Set" 

const string FUNCNAME_RegisterMythicBinEnt = "LootBinReset_ServerToClient_SetMythicBinEnt"
const string FUNCNAME_RegisterGoldBinEnts = "LootBinReset_ServerToClient_SetGoldBinEnts"

const string FUNCNAME_Ping_MythicBinFromMap = "LootBinReset_ClientToServer_Ping_MythicBinFromMap"



global function ServertoClient_LootBinAnnouncement
global function ServerCallback_MythicBinSpawnedClient
global function ServerCallback_UpdateBinPulse

global function ServertoClient_LootBinReset_Warning_Announcement
global function ServertoClient_LootBinReset_Warning_HUD_Indicator_Set

global function LootBinReset_ServerToClient_SetMythicBinEnt
global function LootBinReset_ServerToClient_SetGoldBinEnts

const asset MYTHIC_BIN_ICON = $"rui/hud/gametype_icons/survival/bin_inworld_mythic"  
const asset MYTHIC_BIN_MINIMAP_ICON = $"rui/hud/gametype_icons/survival/bin_inworld_mythic_minimap"


struct
{
	bool lootbinResetTriggered
	entity		  mythicBinEnt

	array< entity > goldBinEnts
















} file



void function Sh_Lootbin_Reset_Init()
{
	if ( !LootbinResetEnabled() )
		return

	Sh_Lootbin_Reset_RegisterNetworking()

	PrecacheParticleSystem( BIN_RESET_FX )

	RegisterSignal( "OpenMythicBin" )
	RegisterSignal( "HaltMythicBinOpening" )


		RegisterMinimapPackage( "prop_script", eMinimapObject_prop_script.MYTHIC_BIN, MINIMAP_OBJECT_RUI, MinimapPackage_MythicBin, FULLMAP_OBJECT_RUI, FullmapPackage_MythicBin )

		if( LootbinReset_PingFromMap_Enabled() )
			AddCallback_OnFindFullMapAimEntity( GetMythicBinUnderAim, PingMythicBinUnderAim )








}




void function Sh_Lootbin_Reset_RegisterNetworking()
{
	Remote_RegisterClientFunction( "ServertoClient_LootBinAnnouncement" )
	Remote_RegisterClientFunction( FUNCNAME_LootBinReset_Warning_Announcement, "float", 0.0, 32000.0, 32 )
	Remote_RegisterClientFunction( FUNCNAME_LootBinReset_Warning_HUD_Indicator_Set, "bool" )
	Remote_RegisterClientFunction( "ServerCallback_MythicBinSpawnedClient", "entity" )
	Remote_RegisterClientFunction( "ServerToClient_MythicBinOpened", "entity" )

	Remote_RegisterClientFunction( FUNCNAME_RegisterMythicBinEnt, "entity" )
	Remote_RegisterClientFunction( FUNCNAME_RegisterGoldBinEnts, "entity" )
	

	Remote_RegisterServerFunction( FUNCNAME_Ping_MythicBinFromMap, "typed_entity", "prop_dynamic" )
}


#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________SERVER___________________________()
{}
#endif











































































































































































































































































































































































































































































































































































































































































































































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________Mythic_Bin___________________________()
{}
#endif











void function MythicBin_OnUse( entity lootbin, entity player, int useInputFlags )
{
	if ( !IsValid( player ) )
	{
		return
	}

	if ( IsBitFlagSet( useInputFlags, USE_INPUT_LONG ) )
	{
		thread MythicBin_UseThink_Thread( lootbin, player )
	}

	else
	{



	}
}

void function MythicBin_UseThink_Thread ( entity lootbin, entity player )
{
	lootbin.EndSignal( "OnDestroy" )

	ExtendedUseSettings settings
	
	
	
	settings.duration = GetPlaylistVar_MythicBinChannelDuration()
	settings.successFunc = MythicBin_ExtendedUseSuccess







		settings.icon = $""
		settings.hint = Localize ( "#MYTHIC_BIN_ACTIVATE_HINT" )
		settings.displayRui = $"ui/extended_use_hint.rpak"
		settings.displayRuiFunc = MythicBin_DisplayRui






	waitthread ExtendedUse( lootbin, player, settings )




}




















void function MythicBin_DisplayRui( entity ent, entity player, var rui, ExtendedUseSettings settings )
{

		RuiSetString( rui, "holdButtonHint", settings.holdHint )
		RuiSetString( rui, "hintText", settings.hint )
		RuiSetGameTime( rui, "startTime", Time() )
		RuiSetGameTime( rui, "endTime", Time() + settings.duration )

}

void function ServerToClient_MythicBinOpened( entity lootbin )
{
	if ( IsValid( lootbin ) )
		lootbin.Signal( "OpenMythicBin" )
}

void function MythicBin_ExtendedUseSuccess( entity lootbin, entity player, ExtendedUseSettings settings )
{
	if ( !IsValid( player ) )
		return

	if ( !IsValid( lootbin ) )
		return















































	lootbin.Signal( "OpenMythicBin" )

}




















































#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________CLIENT___________________________()
{}
#endif


void function PlayWarningSFX()
{
	EmitSoundOnEntity( GetLocalViewPlayer(), "UI_3Strikes_RespawningDisabled" )
}



void function ServertoClient_LootBinAnnouncement()
{
	string message         = Localize( "#LOOTBIN_REFRESH_ANNOUNCEMENT_TITLE" )
	entity localViewPlayer = GetLocalViewPlayer()

	AnnouncementData announcement = Announcement_Create( message )
	announcement.useColorOnSubtext = true
	announcement.announcementStyle = ANNOUNCEMENT_STYLE_GENERIC_WARNING
	announcement.duration          = 7.0

	AnnouncementFromClass( localViewPlayer, announcement )

	thread PlayWarningSFX()
}

void function ServertoClient_LootBinReset_Warning_Announcement( float duration )
{
	thread ServertoClient_LootBinReset_Warning_Announcement_Internal_Thread( duration )
}

void function ServertoClient_LootBinReset_Warning_HUD_Indicator_Set( bool reveal )
{
	string message
	if( reveal )
	{
		if( LootbinReset_Announce_Warning_HudIndicator_UseLongText() )
			message = Localize( "#LOOTBIN_REFRESH_HUD_WARNING_MESSAGE_LONG" )
		else
			message = Localize( "#LOOTBIN_REFRESH_HUD_WARNING_MESSAGE_SHORT" )
	}

	NextCircleClosingStatusChanged( message )
}


void function ServertoClient_LootBinReset_Warning_Announcement_Internal_Thread( float duration )
{
	float timeToWait

	if( LootbinReset_Announce_Warning_NearTrigger_TimeOffset() > 0 )
		timeToWait = duration - LootbinReset_Announce_Warning_NearTrigger_TimeOffset()
	else
		timeToWait = 20.0

	wait timeToWait

	string message         = Localize( "#LOOTBIN_REFRESH_WARNING_ANNOUNCEMENT" )
	entity localViewPlayer = GetLocalViewPlayer()

	AnnouncementData announcement = Announcement_Create( message )
	announcement.useColorOnSubtext = true
	announcement.announcementStyle = ANNOUNCEMENT_STYLE_GENERIC_WARNING
	announcement.duration          = 5.0

	AnnouncementFromClass( localViewPlayer, announcement )

	thread PlayWarningSFX()
}



void function ServerCallback_MythicBinSpawnedClient( entity binFromServer )
{
	AddCallback_OnUseEntity_ClientServer( binFromServer, MythicBin_OnUse )
	thread Bin_CreateClientSideHUDMarker_thread( MYTHIC_BIN_ICON, binFromServer, 12, true )
}



void function ServerCallback_UpdateBinPulse( entity binFromServer )
{
	binFromServer.kv.rendercolor = "255 255 255"
}



var function Bin_CreateClientSideHUDMarker_thread ( asset hudImage, entity lootbin, float upOffset, bool trackPosition )
{
	lootbin.EndSignal( "OnDestroy" )
	lootbin.EndSignal( "OnDeath" )
	lootbin.EndSignal( "OpenMythicBin" )

	entity localViewPlayer = GetLocalViewPlayer()
	vector pos             = lootbin.GetOrigin()
	var rui                = CreateFullscreenRui( PERK_IN_WORLD_HUD_OBJECT, RuiCalculateDistanceSortKey( localViewPlayer.EyePosition(), pos ) )
	RuiSetImage( rui, "beaconImage", hudImage )

	printf( "Mythic Bin Color: " + SrgbToLinear( GetKeyColor( COLORID_MYTHIC_BIN_ICON ) / 255.0 ) )

	RuiSetFloat3( rui, "iconColor", SrgbToLinear( GetKeyColor( COLORID_MYTHIC_BIN_ICON ) / 255.0 ) )


	RuiSetGameTime( rui, "startTime", Time() )
	if( trackPosition )
	{
		RuiTrackFloat3( rui, "pos", lootbin, RUI_TRACK_ABSORIGIN_FOLLOW  )
	}
	else
	{
		RuiSetFloat3( rui, "pos", pos )
	}

	RuiSetFloat( rui, "upOffset", upOffset )
	RuiKeepSortKeyUpdated( rui, true, "pos" )
	RuiSetBool( rui, "isVisible", true )


	OnThreadEnd(
		function() : ( rui )
		{
			if ( IsValid( rui ) )
				RuiSetBool( rui, "isVisible", false )
		}
	)

	WaitForever()
}

void function LootBinReset_ServerToClient_SetMythicBinEnt( entity lootBin )
{
	if ( !IsValid( lootBin ) )
		return

	file.mythicBinEnt = lootBin
}

void function LootBinReset_ServerToClient_SetGoldBinEnts( entity lootBin )
{
	if ( !IsValid( lootBin ) )
		return

	file.goldBinEnts.append( lootBin )
}



void function MinimapPackage_MythicBin( entity ent, var rui )
{
	RuiSetImage( rui, "defaultIcon", MYTHIC_BIN_MINIMAP_ICON )
	RuiSetImage( rui, "clampedDefaultIcon", $"" )
	RuiSetFloat3( rui, "iconColor", SrgbToLinear( GetKeyColor( COLORID_MYTHIC_BIN_ICON ) / 255.0 ) )

	RuiSetBool( rui, "useTeamColor", false )

	RuiSetImage( rui, "smallIcon", MYTHIC_BIN_MINIMAP_ICON )
	RuiSetBool( rui, "hasSmallIcon", true )

}

void function FullmapPackage_MythicBin( entity ent, var rui )
{
	RuiSetImage( rui, "defaultIcon", MYTHIC_BIN_ICON )
	RuiSetImage( rui, "clampedDefaultIcon", $"" )
	RuiSetFloat3( rui, "iconColor", SrgbToLinear( GetKeyColor( COLORID_MYTHIC_BIN_ICON ) / 255.0 ) )

	RuiSetBool( rui, "useTeamColor", false )

	RuiSetImage( rui, "smallIcon", MYTHIC_BIN_ICON )
	RuiSetBool( rui, "hasSmallIcon", true )
}


#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________PlaylistVars___________________________()
{}
#endif

int function GetPlaylistVar_LootbinRefreshDeathstageCloseTrigger()
{
	return GetCurrentPlaylistVarInt( "lootbin_reset_deathstage_trigger", 1 )
}

int function GetPlaylistVar_LootbinRefreshMaxGoldBins()
{
	return GetCurrentPlaylistVarInt( "lootbin_reset_gold_bins_maximum", 8 )
}

float function GetPlaylistVar_LootbinRefreshGoldBinSpawnDistance()
{
	return GetCurrentPlaylistVarFloat( "lootbin_reset_gold_bin_spawn_distance", 2000 )  
}

float function GetPlaylistVar_MythicBinChannelDuration()
{
	return GetCurrentPlaylistVarFloat( "lootbin_reset_mythic_channel_duration", 11.0 )
}

bool function LootbinResetEnabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_enabled", true )
}

bool function LootbinReset_UseUniqueSpawnSources_Enabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_use_unique_spawn_sources_enabled", true )
}

bool function LootbinReset_AllowPerkBinReuse_Enabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_perkbin_reuse_enabled", true )
}

bool function LootbinReset_PingFromMap_Enabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_ping_from_map_enabled", true )
}

bool function LootbinReset_UpdatedGroundLoot_Enabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_updated_groundloot_enabled", true )
}

bool function LootbinReset_Announce_VO_Enabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_announce_vo_enabled", true )
}

bool function LootbinReset_Announce_OnResetTrigger_Enabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_announce_on_reset_trigger_enabled", false )
}

bool function LootbinReset_Announce_Warning_NearTrigger_Enabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_announce_near_reset_trigger_enabled", true )
}

float function LootbinReset_Announce_Warning_NearTrigger_TimeOffset()
{
	return GetCurrentPlaylistVarFloat( "lootbin_reset_announce_near_reset_time_offset", 50 )
}

bool function LootbinReset_Announce_Warning_HUD_OnTriggerRoundStart_Enabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_announce_warning_hud_on_trigger_round_enabled", true )
}

bool function LootbinReset_Announce_Warning_HudIndicator_UseLongText()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_announce_warning_hud_indicator_uselongtext", false )
}

bool function LootbinReset_ReSkin_UsedPerkBins_Enabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_reskin_used_perkbins_enabled", true )
}

bool function LootbinReset_Deflated_ResetLootPools_Enabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_deflated_resetlootpools_enabled", false )
}

bool function GetPlaylistVar_MythicBinsEnabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_mythic_bins_enabled", true )
}

bool function GetPlaylistVar_GoldBinsEnabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_gold_bins_enabled", true )
}

bool function GetPlaylistVar_GoldBins_UseSmartLoot()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_gold_bins_reward_smart_loot", true )
}

int function GetPlaylistVar_GoldBins_NumOfItemsToSpawn()
{
	return GetCurrentPlaylistVarInt( "lootbin_reset_gold_bins_num_items_to_spawn", 6 )
}

bool function GetPlaylistVar_MythicBins_ReawrdDynamicLoot()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_mythic_bins_reward_dynamic_loot", true )
}

array<string> function LootBinReset_MythicBins_GetStaticRewards()
{
	array<string> rewards

	string defaults = GetCurrentPlaylistVarString( "lootbin_reset_mythic_bins_static_loot_rewards", "mp_pickup_xp_cache_dynamic mp_weapon_sniper mp_pickup_xp_cache_dynamic mp_weapon_esaw_crate mp_pickup_xp_cache_dynamic" )

	if( GetCurrentPlaylistVarString( "pin_match_type", "survival" ) == "duo" )
	{
		defaults = GetCurrentPlaylistVarString( "lootbin_reset_mythic_bins_static_loot_rewards_duos", "mp_pickup_xp_cache_dynamic mp_weapon_sniper mp_pickup_xp_cache_dynamic mp_weapon_esaw_crate" )
	}

	rewards = GetTrimmedSplitString( defaults, " " )

	return rewards
}

bool function GetPlaylistVar_MythicBins_AwardTeamEvoOnOpen()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_mythic_bins_award_evo_onopen", true )
}

int function LootBinReset_MythicBins_OnOpen_MinEvo_Reward()
{
	return GetCurrentPlaylistVarInt( "lootbin_reset_mythic_bins_onopen_min_evo_award", 500 )
}

bool function LootBinReset_MythicBins_OnOpen_ConfettiLootEnabled()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_mythic_bins_pop_lootconfetti", true )
}

int function LootBinReset_MythicBins_OnOpen_ConfettiLootCount()
{
	return GetCurrentPlaylistVarInt( "lootbin_reset_mythic_bins_confetti_count", 6 )
}

int function LootBinReset_MythicBins_OnOpen_ConfettiLoot_SwitchThreshold()
{
	return GetCurrentPlaylistVarInt( "lootbin_reset_mythic_bins_confetti_switchThreshold", 3 )
}

int function GetPlaylistVar_MythicBins_MinItemCount()
{
	return GetCurrentPlaylistVarInt( "lootbin_reset_mythic_bins_min_item_count", 5 )
}



bool function LootBinReset_LootBinsHaveReset()
{
	return file.lootbinResetTriggered
}

bool function LootBinReset_MythicBins_HaveUniqueInteractPrompt()
{
	return GetCurrentPlaylistVarBool( "lootbin_reset_mythic_bins_unique_use_prompt", true )
}


bool function LootBinReset_IsLootBinSpecial( entity lootBin )
{
	if( lootBin == file.mythicBinEnt )
		return true

	foreach( bin in file.goldBinEnts )
	{
		if( bin == lootBin )
			return true
	}

	return false
}

bool function LootBinReset_IsLootBin_Mythic( entity lootBin )
{
	if( lootBin == file.mythicBinEnt )
		return true

	return false
}

bool function LootBinReset_IsLootBin_GoldBin( entity lootBin )
{
	
	
	
	
	

	foreach( bin in file.goldBinEnts )
	{
		if( bin == lootBin )
			return true
	}

	return false
}





















entity function GetMythicBinUnderAim( vector worldPos, float worldRange )
{
	float closestDistSqr        = FLT_MAX
	float worldRangeSqr = worldRange * worldRange
	float extendedRange = worldRange * 2
	entity closestEnt = null

	entity player = GetLocalViewPlayer()

	entity binEnt = file.mythicBinEnt

	if( !IsValid( binEnt ) )
		return null

	if( MapPing_Modify_DistanceCheck_Enabled() )
	{
		float modifier = MapPing_DistanceCheck_GetModifier()

		if( worldRange >= MapPing_DistanceCheck_GetDistanceRange() )
			modifier *= 0.5

		worldRangeSqr = ( worldRange * modifier ) * ( worldRange * modifier )
	}

	vector binPos = file.mythicBinEnt.GetOrigin()

	float distSqr = Distance2DSqr( binPos, worldPos )
	if ( distSqr < ( worldRangeSqr * 2 ) && distSqr < closestDistSqr  )
	{
		closestDistSqr = distSqr
		closestEnt     = binEnt
	}


	if ( !IsValid( closestEnt ) )
	{
		return null
	}

	return closestEnt
}

bool function PingMythicBinUnderAim( entity binEnt )
{
	entity player = GetLocalClientPlayer()

	if ( !IsValid( player ) || !IsAlive( player ) )
		return false

	if ( !IsPingEnabledForPlayer( player ) )
		return false

	if( LootbinReset_PingFromMap_Enabled() )
		Remote_ServerCallFunction( FUNCNAME_Ping_MythicBinFromMap, binEnt ) 

	EmitSoundOnEntity( GetLocalViewPlayer(), PING_SOUND_LOCAL_CONFIRM )

	return true
}


#if DEV
#if INTELLIJ_OUTLINE_SECTION_MARKER
void function _____________DEV___________________________()
{}
#endif





















































































#endif

      