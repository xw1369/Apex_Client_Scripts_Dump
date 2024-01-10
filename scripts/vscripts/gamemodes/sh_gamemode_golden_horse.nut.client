

global function GoldenHorse_Init

global function IsGoldenHorse
global function IsGoldenHorse_FR
global function GoldenHorse_TicksEnabled
global function GoldenHorse_SwordCarePackageEnabled
global function GoldenHorse_ModifyCarePackageSpawnDistance














global function ServerToClient_GoldenHorse_RegisterLootTick
global function ServerToClient_GoldenHorse_MarkAllLootTicks

global function GoldenHorse_UpdateLootHighlight



const string PVAR_GOLDEN_HORSE_ENABLED = "is_golden_horse"
const string PVAR_GOLDEN_HORSE_FR_ENABLED = "is_golden_horse_fr"
const string PVAR_GOLDEN_HORSE_TICKS_ENABLED = "golden_horse_ticks_enabled"
const string PVAR_GOLDEN_HORSE_SWORD_SPAWNS_ENABLED = "golden_horse_swords_enabled"
const string PVAR_GOLDEN_HORSE_SWORD_HOVERTANK_ENABLED = "golden_horse_sword_hovertank_enabled"
const string PVAR_GOLDEN_HORSE_SWORD_CAREPACKAGE_ENABLED = "golden_horse_sword_carepackage_enabled"
const string PVAR_GOLDEN_HORSE_CAREPACKAGE_DIST_ENABLED = "golden_horse_carepackage_dist_enabled"
const string PVAR_GOLDEN_HORSE_SPAWN_ALL_TICKS = "golden_horse_spawn_all_ticks"
const string PVAR_GOLDEN_HORSE_TICKS_PER_CLUSTER = "golden_horse_ticks_per_cluster"
const string PVAR_GOLDEN_HORSE_SWORDS_PER_CLUSTER = "golden_horse_swords_per_cluster"
const string PVAR_GOLDEN_HORSE_SWORDS_SUBDIVIDE_PER_CLUSTER = "golden_horse_swords_subdivide"
const string PVAR_GOLDEN_HORSE_SPAWN_ALL_SWORDS = "golden_horse_spawn_all_swords"
const string PVAR_GOLDEN_HORSE_RED_MAX = "golden_horse_red_max"
const string PVAR_GOLDEN_HORSE_TICK_DIST_HUD_MIN = "golden_horse_tick_dist_hud_min"
const string PVAR_GOLDEN_HORSE_TICK_DIST_HUD_MAX = "golden_horse_tick_dist_hud_min"


global const string GOLDEN_HORSE_LOOT_TICK_SCRIPT_NAME = "loot_tick_golden_horse"


const int GOLDEN_HORSE_TICKS_PER_CLUSTER = 4 
const int GOLDEN_HORSE_SWORDS_PER_CLUSTER = 3 
const int GOLDEN_HORSE_SWORDS_SUBDIVIDE = 1 
const int GOLDEN_HORSE_RED_MAX = 60
global const int GOLDEN_HORSE_SPECIAL_EVENT_LOOT_TIER = 107 
const float GOLDEN_HORSE_TICK_DIST_HUD_MIN = 30 * METERS_TO_INCHES
const float GOLDEN_HORSE_TICK_DIST_HUD_MAX = 35 * METERS_TO_INCHES


const asset GOLDEN_HORSE_TICK_ICON = $"rui/gamemodes/golden_horse/golden_horse_tick_icon"

struct ClusterSpawnData
{
	vector origin
	vector angles
}

struct
{







		table<entity, var> tickMarkersTable
		table<entity, var> tickWorldIconTable 

} file


bool function IsGoldenHorse()
{
	return GetCurrentPlaylistVarBool( PVAR_GOLDEN_HORSE_ENABLED, false )
}

bool function IsGoldenHorse_FR()
{
	return GetCurrentPlaylistVarBool( PVAR_GOLDEN_HORSE_FR_ENABLED, false )
}

bool function GoldenHorse_SwordCarePackageEnabled()
{
	return IsGoldenHorse() && GetCurrentPlaylistVarBool( PVAR_GOLDEN_HORSE_SWORD_CAREPACKAGE_ENABLED, true )
}

bool function GoldenHorse_ModifyCarePackageSpawnDistance()
{
	return IsGoldenHorse() && GetCurrentPlaylistVarBool( PVAR_GOLDEN_HORSE_CAREPACKAGE_DIST_ENABLED, true )
}

bool function GoldenHorse_TicksEnabled()
{
	return IsGoldenHorse() && GetCurrentPlaylistVarBool( PVAR_GOLDEN_HORSE_TICKS_ENABLED, true )
}

bool function GoldenHorse_DebugTicks()
{
	return GetCurrentPlaylistVarBool( PVAR_GOLDEN_HORSE_SPAWN_ALL_TICKS, false )
}

bool function GoldenHorse_SwordSpawnsEnabled()
{
	return GetCurrentPlaylistVarBool( PVAR_GOLDEN_HORSE_SWORD_SPAWNS_ENABLED, true )
}

bool function GoldenHorse_DebugSwordSpawns()
{
	return GetCurrentPlaylistVarBool( PVAR_GOLDEN_HORSE_SPAWN_ALL_SWORDS, false )
}

void function GoldenHorse_Init()
{
	if ( !IsGoldenHorse() )
		return











		AddCallback_OnPlayerMatchStateChanged( GoldenHorse_Client_OnPlayerMatchStateChanged )
		AddCallback_GameStateEnter( eGameState.Playing, GoldenHorse_OnGamestateEnterPlaying_Client )
		AddCallback_OnFindFullMapAimEntity( GetLootTickUnderAim, PingLootTickUnderAim )
		GoldenHorse_RegisterMinimapPackage()



		Remote_RegisterClientFunction( "ServerToClient_GoldenHorse_RegisterLootTick", "entity" )
		Remote_RegisterClientFunction( "ServerToClient_GoldenHorse_MarkAllLootTicks" )
		Remote_RegisterServerFunction( "ClientToServer_ClientToServer_PingLootTickFromMap", "typed_entity", "npc_frag_drone" )

}

void function OnEntitiesDidLoad()
{







}
























































































































































void function ServerToClient_GoldenHorse_RegisterLootTick( entity tick )
{
	if ( !IsValid( tick ) )
		return

	AddEntityDestroyedCallback( tick,
		void function( entity ent ) : ( tick )
		{
			GoldenHorse_OnTickDestroyed( tick )
		}
	)

	vector pos             = tick.GetOrigin() + tick.GetUpVector() * 50
	entity localViewPlayer = GetLocalViewPlayer()
	var rui                = GoldenHorse_CreateTickRui( pos, 36, 48,
		GetCurrentPlaylistVarFloat( PVAR_GOLDEN_HORSE_TICK_DIST_HUD_MIN, GOLDEN_HORSE_TICK_DIST_HUD_MIN ), GetCurrentPlaylistVarFloat( PVAR_GOLDEN_HORSE_TICK_DIST_HUD_MAX, GOLDEN_HORSE_TICK_DIST_HUD_MAX ), true )

	file.tickWorldIconTable[ tick ] <- rui
}

void function GoldenHorse_OnTickDestroyed( entity tick )
{
	if ( !(tick in file.tickWorldIconTable) )
		return

	var rui = file.tickWorldIconTable[ tick ]
	RuiSetBool( rui, "isFinished", true )
}

void function GoldenHorse_Client_OnPlayerMatchStateChanged( entity player, int newState )
{
	if ( player != GetLocalViewPlayer() )
		return

	
	if ( newState == ePlayerMatchState.SKYDIVE_PRELAUNCH )
	{
		GoldenHorse_PromptMarkAllLootTicks()
	}

	
	if ( newState == ePlayerMatchState.NORMAL )
	{
		GoldenHorse_DestroyLootTickMarkers()
	}
}

void function GoldenHorse_PromptMarkAllLootTicks()
{
	thread Thread_PromptMarkAllLootTicks()
}

void function Thread_PromptMarkAllLootTicks()
{
	entity player = GetLocalClientPlayer()
	if ( !IsValid( player ) || ShouldMuteCommsActionForCooldown( player, eCommsAction.QUICKCHAT_GH_MARK_LOOT_TICKS, null ) )
		return

	wait 2.0

	AddOnscreenPromptFunction( "quickchat", CreateQuickchatFunction( eCommsAction.QUICKCHAT_GH_MARK_LOOT_TICKS, player ), 10, Localize( "#QUICKCHAT_MARK_ALL_CACTICKS" ) )
}

void function ServerToClient_GoldenHorse_MarkAllLootTicks()
{
	entity localViewPlayer = GetLocalViewPlayer()
	EmitSoundOnEntity( localViewPlayer, "coop_minimap_ping" )

	foreach ( entity tick, var icon in file.tickWorldIconTable )
	{
		vector pos = tick.GetOrigin() + tick.GetUpVector() * 50
		var rui    = GoldenHorse_CreateTickRui( pos, 32, 54, 50000, 200000, false )
		file.tickMarkersTable[tick] <- rui
	}
}

var function GoldenHorse_CreateTickRui( vector pos, float sizeMin, float sizeMax, float minDist, float maxDist, bool hideNearCrossHair )
{
	entity localViewPlayer = GetLocalViewPlayer()
	var rui                = CreatePermanentCockpitRui( $"ui/survey_beacon_marker_icon.rpak", RuiCalculateDistanceSortKey( localViewPlayer.EyePosition(), pos ) )
	RuiSetImage( rui, "beaconImage", GOLDEN_HORSE_TICK_ICON )
	RuiSetGameTime( rui, "startTime", Time() )
	RuiSetFloat3( rui, "pos", pos )
	RuiSetFloat( rui, "sizeMin", sizeMin )
	RuiSetFloat( rui, "sizeMax", sizeMax )
	RuiSetFloat( rui, "minAlphaDist", minDist )
	RuiSetFloat( rui, "maxAlphaDist", maxDist )
	RuiSetBool( rui, "shouldHideNearCrosshairs", hideNearCrossHair )
	RuiKeepSortKeyUpdated( rui, true, "pos" )

	return rui
}

void function GoldenHorse_DestroyLootTickMarkers()
{
	foreach ( rui in file.tickMarkersTable )
	{
		RuiSetBool( rui, "isFinished", true )
	}
	file.tickMarkersTable.clear()
}











































































string function GoldenHorse_UpdateLootHighlight( entity prop, string ref, int tier, string highlight )
{
	if ( !IsGoldenHorse() )
		return highlight

	if ( ref == "hopup_golden_horse_green" )
		highlight = "survival_item_gh"

	return highlight
}




void function GoldenHorse_OnGamestateEnterPlaying_Client()
{
	SetMapFeatureItem( 1000, "#GOLDEN_HORSE_MAP_FEATURE", "#GOLDEN_HORSE_MAP_FEATURE_DESC", GOLDEN_HORSE_TICK_ICON )
}

void function GoldenHorse_RegisterMinimapPackage()
{
	RegisterMinimapPackage( "npc_frag_drone", eMinimapObject_npc.LOOT_TICK_GH, MINIMAP_OBJECT_RUI, MinimapPackage_LootTick, FULLMAP_OBJECT_RUI, FullmapPackage_LootTick )
}

void function MinimapPackage_LootTick( entity ent, var rui )
{
	RuiSetImage( rui, "defaultIcon", GOLDEN_HORSE_TICK_ICON )
	RuiSetImage( rui, "clampedDefaultIcon", $"" )
	RuiSetBool( rui, "useTeamColor", false )
	RuiSetBool( rui, "forceShow", true )
}

void function FullmapPackage_LootTick( entity ent, var rui )
{
	RuiSetImage( rui, "defaultIcon", GOLDEN_HORSE_TICK_ICON )
	RuiSetImage( rui, "clampedDefaultIcon", $"" )
	RuiSetBool( rui, "useTeamColor", false )
	RuiSetBool( rui, "forceShow", true )
}

entity function GetLootTickUnderAim( vector worldPos, float worldRange )
{
	float closestDistSqr = FLT_MAX
	float worldRangeSqr  = worldRange * worldRange
	entity closestEnt    = null

	if ( MapPing_Modify_DistanceCheck_Enabled() )
	{
		float modifier = MapPing_DistanceCheck_GetModifier()

		if ( worldRange >= MapPing_DistanceCheck_GetDistanceRange() )
			modifier *= 0.5

		worldRangeSqr = (worldRange * modifier) * (worldRange * modifier)
	}

	foreach ( entity tick, var rui in file.tickWorldIconTable )
	{
		if ( !IsValid( tick ) )
			continue

		vector beaconOrigin = tick.GetOrigin()

		float distSqr = Distance2DSqr( beaconOrigin, worldPos )
		if ( distSqr < worldRangeSqr && distSqr < closestDistSqr )
		{
			closestDistSqr = distSqr
			closestEnt     = tick
		}
	}

	if ( !IsValid( closestEnt ) )
		return null

	return closestEnt
}

bool function PingLootTickUnderAim( entity tick )
{
	entity player = GetLocalClientPlayer()

	if ( !IsValid( player ) || !IsAlive( player ) || !IsPingEnabledForPlayer( player ) )
		return false

	Remote_ServerCallFunction( "ClientToServer_ClientToServer_PingLootTickFromMap", tick )
	EmitSoundOnEntity( GetLocalViewPlayer(), PING_SOUND_LOCAL_CONFIRM )
	return true
}


















































































































































































































































































































































































































































































#if DEV





















#endif

                         