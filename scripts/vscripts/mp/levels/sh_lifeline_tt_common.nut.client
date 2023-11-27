
global function LifelineTT_Init
global function IsLifelineTTEnabled
global function GetLifelineTTAssetsToPrecache


const string LIFELINETT_PLAYLIST_ENABLE_MEDKIT_SPAWNS = "lifeline_tt_medkit_spawns"
const string LIFELINETT_PLAYLIST_ENABLE_JAMROOM = "lifeline_tt_jamroom_enabled"
const string LIFELINETT_PLAYLIST_ENABLE_INSTRUMENTS = "lifeline_tt_jamroom_use_enabled"
const string LIFELINETT_PLAYLIST_SMARTDROP_MODE = "lifeline_tt_smartdrop_mode"
const string LIFELINETT_PLAYLIST_SMARTDROP_DELAY_SEC = "lifeline_tt_smartdrop_delay"
const string LIFELINETT_PLAYLIST_ENABLE_CENTER_LOOT = "lifeline_tt_center_loot"


const string LIFELINETT_LOOT_KEYWORD = "lifeline_tt_loot"
const string LIFELINETT_CENTER_LOOT_SCRIPTNAME = "lifeline_tt_medbay_loot"


enum eSmartDropMode
{
	DISABLED,
	SINGLE,
	MULTI,
	AUTO_SINGLE,
	AUTO_MULTI
}

const string CONSOLE_SCRIPT_NAME = "lifeline_tt_smartdrop_console"
const string SMARTDROP_LOCATION_SCRIPT_NAME = "lifeline_tt_smartdrop_location"

const float SMARTDROP_DELAY_SEC = 5


enum eJamRoomInstruments
{
	GUITAR = 0,
	BASS = 1,
	DRUMS = 2
}

const string JAMROOM_INSTRUMENT_SCRIPT_NAME = "jamroom_instrument"

const string JAMROOM_GUITAR_TARGET_NAME = "jamroom_guitar"
const string JAMROOM_BASS_TARGET_NAME = "jamroom_bass"
const string JAMROOM_DRUMS_TARGET_NAME = "jamroom_drums"

const string MDL_JAMROOM_DRUMS = "mdl/olympus/olympus_ll_tt_prop_wall_drums_01.rmdl"


const string JAMROOM_MUSIC_SCRIPT_NAME = "lifeline_tt_jamroom_music"
const string JAMROOM_MUSIC_SPLINE_SCRIPT_NAME = "Lifeline_TT_Emitter"
const string JAMROOM_MUSIC_CODE_CONTROLLER_SCRIPT_NAME = "lifeline_tt_music_controller"

const int JAMROOM_AMBIENT_CONTROL = 0
const int JAMROOM_DRUM_CONTROL = 100
const int JAMROOM_BASS_CONTROL = 125
const int JAMROOM_GUITAR_CONTROL = 150
const float JAMROOM_STEM_RESET_TIME_SEC = 15

const int JAMROOM_DRUM_USABLE_DIST = 64
const int JAMROOM_GUITAR_USABLE_DIST = 64
const int JAMROOM_BASS_USABLE_DIST = 64

const table<int, int> JAMROOM_INSTRUMENT_STEMS = {
	[eJamRoomInstruments.GUITAR] = JAMROOM_GUITAR_CONTROL,
	[eJamRoomInstruments.BASS] = JAMROOM_BASS_CONTROL,
	[eJamRoomInstruments.DRUMS] = JAMROOM_DRUM_CONTROL
}

struct InstrumentData
{
	int instrumentType
}


const string SFX_CONSOLE_USE_1P = "Canyonlands_Scr_Pilon_Initiate"
const string SFX_CONSOLE_USE_3P = "Canyonlands_Scr_Pilon_Initiate_3P"

const string SFX_JAMROOM_MUSIC = "music_tt_lifeline_jamroom"

const string SFX_JAMROOM_GUITAR = "Olympus_LifelineTT_JamRoom_Guitar_Interact"
const string SFX_JAMROOM_BASS = "Olympus_LifelineTT_JamRoom_Bass_Interact"
const string SFX_JAMROOM_DRUMS = "Olympus_LifelineTT_JamRoom_Drums_Interact"

const table<int, string> JAMROOM_INSTRUMENT_SFX = {
	[eJamRoomInstruments.GUITAR] = SFX_JAMROOM_GUITAR,
	[eJamRoomInstruments.BASS] = SFX_JAMROOM_BASS,
	[eJamRoomInstruments.DRUMS] = SFX_JAMROOM_DRUMS
}

struct SmartDropData
{
	vector origin
	vector angles
}

struct
{








} file

bool function GetLifelineTTAssetsToPrecache( array< string > models, array< string > particles )
{
	if ( IsLifelineTTEnabled() == false )
	{
		return false
	}

	
	models.append( MDL_JAMROOM_DRUMS )

	
	models.append( MDL_MEDBAY_EYE_JOINT )
	models.append( MDL_MEDBAY_EYE_STALK )
	models.append( MDL_MEDBAY_RING )

	particles.append( VFX_MEDBAY_HEAL_COCKPIT_1P )
	particles.append( VFX_MEDBAY_PLATFORM )
	particles.append( VFX_MEDBAY_SPARKS )
	particles.append( VFX_MEDBAY_CROSS )
	particles.append( VFX_MEDBAY_CROSS_PULSE )

	return true
}

void function LifelineTT_Init()
{





		AddCreateCallback( "prop_dynamic", JamRoom_OnPropDynamicCreated )

}

void function EntitiesDidLoad()
{
	if ( !IsLifelineTTEnabled() )
		return

	PrecacheScriptString( JAMROOM_MUSIC_CODE_CONTROLLER_SCRIPT_NAME )
	PrecacheScriptString( GENERIC_PING_PANEL_SCRIPTNAME )
	PrecacheScriptString( GENERIC_PING_PANEL_SCRIPTNAME_INACTIVE )







}














































































































































































void function JamRoom_OnPropDynamicCreated( entity ent )
{
	if ( ent.GetScriptName() == JAMROOM_MUSIC_CODE_CONTROLLER_SCRIPT_NAME )
		JamRoom_InitClientAmbient( ent )
}

void function JamRoom_InitClientAmbient( entity controller )
{
	if ( IsValid( controller ) )
	{
		array<entity> ambientGenerics = GetEntArrayByScriptName( JAMROOM_MUSIC_SPLINE_SCRIPT_NAME )
		if ( ambientGenerics.len() == 1 )
		{
			entity ambient = ambientGenerics[0]
			ambient.SetSoundCodeControllerEntity( controller )
		}
	}
}































































































































































































bool function IsLifelineTTEnabled()
{
	if ( GetCurrentPlaylistVarBool( "lifeline_tt_enabled", true ) )
	{



			return HasEntWithScriptName( JAMROOM_MUSIC_SPLINE_SCRIPT_NAME )

	}

	return false
}

