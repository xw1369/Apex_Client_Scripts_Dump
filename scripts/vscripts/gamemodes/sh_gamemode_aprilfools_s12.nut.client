

global function AprilFools_S12_Mode_Init
global function AprilFools_S12_Map_Init
global function IsAprilFoolsNessieSwap

#if DEV



#endif

global const string APRIL_FOOLS_12_PLAYLIST_VAR = "is_april_fools_s12"
global const string APRIL_FOOLS_SPIDER_LIFETIME_PLAYLIST_VAR = "april_fools_spider_lifetime"
global const string APRIL_FOOLS_WATTSON_LAUGH_PLAYLIST_VAR = "april_fools_wattson_laugh"

const string APRIL_FOOLS_NESSIE_SWAP_PLAYLIST_VAR = "april_fools_nessie"
const string APRIL_FOOLS_SPAWN_EGGS_PLAYLIST_VAR = "april_fools_spawn_eggs"
const string APRIL_FOOLS_ALLOW_NESSIE_CHARMS_PLAYLIST_VAR = "april_fools_allow_nessie_charms"
const string APRIL_FOOLS_DISABLE_MAPS_PLAYLIST_VAR = "april_fools_disable_maps"
const string APRIL_FOOLS_SPAWN_ALL_EGGS_PLAYLIST_VAR = "april_fools_debug_all_eggs"
const string APRIL_FOOLS_CONFETTI_ENABLED_PLAYLIST_VAR = "april_fools_enable_confetti"

const int APRIL_FOOLS_SHOTGUN_EGG_COUNT = 8
const vector APRIL_FOOLS_EGG_BEAM_OFFSET = <3, 20, 0>

const string APRIL_FOOLS_SHOTGUN = "mp_weapon_shotgun_pistol_april_fools"
const string CHARACTER_WATTSON = "character_wattson"


const string SFX_EGG_LOOP = "AFLTM_Egg_AttractLoop"
const string SFX_EGG_AF_EXPLODE = "AFLTM_EggBurst"






struct{







}file

bool function IsAprilFools_S12()
{
	return GetCurrentPlaylistVarBool( APRIL_FOOLS_12_PLAYLIST_VAR, false )
}

bool function IsAprilFoolsNessieSwap()
{
	return IsAprilFools_S12() && IsNessieSwap()
}

bool function IsNessieSwap()
{
	return GetCurrentPlaylistVarBool( APRIL_FOOLS_NESSIE_SWAP_PLAYLIST_VAR, true )
}

bool function DebugAllEggs()
{
	return GetCurrentPlaylistVarBool( APRIL_FOOLS_SPAWN_ALL_EGGS_PLAYLIST_VAR, false )
}

bool function IsValidGameMode()
{
	return !IsRankedGame()
}

bool function IsValidMap( string map )
{
	array<string> disabledMaps = split( GetCurrentPlaylistVarString( APRIL_FOOLS_DISABLE_MAPS_PLAYLIST_VAR, "" ), "," )
	return !disabledMaps.contains( map )
}

void function AprilFools_S12_Map_Init(bool areSpidersAlreadyLoaded)
{
	PrecacheScriptString( SPIDER_EGG_SCRIPT_NAME )











}

void function AprilFools_S12_Mode_Init()
{
	if ( !IsAprilFools_S12() )
		return

	if ( !IsValidGameMode() )
		return


















}



































































































































































































































































































































      