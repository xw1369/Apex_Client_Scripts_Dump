
global function ShFiringRangeStoryEvents_Init


struct RealmStoryData
{
	entity door
}

struct
{
	table< int,  RealmStoryData > realmStoryDataByRealmTable
} file

const array<string> BTS_DIALOGUE_ARRAY = [ ]

const asset BTS_DOOR_MDL = $"mdl/door/metal_swinging_door_01.rmdl"
const string BTS_DOOR_SCRIPT_NAME = "FR_BTS_DOOR_SCRIPTNAME"




















void function ShFiringRangeStoryEvents_Init()
{
	if ( GetMapName() != "mp_rr_canyonlands_staging_mu1" ) 
		return

	if ( !GameModeVariant_IsActive( eGameModeVariants.SURVIVAL_FIRING_RANGE ) )
		return

	PrecacheScriptString( BTS_DOOR_SCRIPT_NAME )

	AddCallback_EntitiesDidLoad( EntitiesDidLoad )






	RegisterSignal( "EndBTSConvo" )
}



void function EntitiesDidLoad()
{

}








































































































































































