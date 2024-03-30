#if DEV
global function SpawnDebugger_Init
global function DEV_EnableSpawnPointTesting








const string SPAWN_DEBUG_SIGNAL = "ShowOnScreenDebug"
const string SPAWN_DEBUG_MATCHING_POS = "PLAYER POSITION DOES NOT MATCH SPAWN POINT POSITION"
const string DEBUG_FILE_OUTPUT_PATH = "../../dumps/" 

const vector SPAWN_DEBUG_COLOR_GREEN = < 0, 255, 0 > 
const vector SPAWN_DEBUG_COLOR_RED = < 255, 0, 0 > 
const vector TRACE_HULL_MIN = < -16, -16, 0 >
const vector TRACE_HULL_MAX = < 16, 16, 80 >
const vector DEBUG_BOX_CENTER_OFFSET = < 0, 0, 16 >

const float MAX_SPAWNPOINT_HEIGHT_FROM_NAVMESH = 16.01 
const float TRACE_LENGTH = 80 
const float DEBUG_SPHERE_RADIUS = 8.0
const float DEBUG_DRAW_DURATION = 120.0

const int MAX_SPAWNPOINT_TO_DEBUG_DRAW = 20 

struct
{
	int spawnPointInt = 0
	vector spawnPointOrigin
	string mapName
	array<vector> spawnPointPosArray
	array<entity> spawnPointOnNavMeshArray
	array<entity> spawnPointHeightCheckArray
	float distanceToNavMesh
} file
#endif





#if DEV
void function SpawnDebugger_Init()
{
	RegisterSignal( SPAWN_DEBUG_SIGNAL )
}

void function DEV_EnableSpawnPointTesting( bool enabled)
{
	entity player = GP()
	Assert( IsValid( player ), "Unable to find a local player: DEV_EnableSpawnPointTesting" )

	if ( enabled )
	{





			player.ClientCommand( "bind KP_RIGHTARROW \"script DEV_ForceSpawnAtNextSpawnPoint()\"" )
			player.ClientCommand( "bind KP_LEFTARROW \"script DEV_ForceSpawnAtPreviousSpawnPoint()\"" )
			player.ClientCommand( "bind KP_DOWNARROW \"script DEV_PrintSpawnPointLocationToFile()\"" )
			player.ClientCommand( "bind KP_5 \"script DEV_SaveSpawnPointLocation()\"" )
			printt( "Enabled Spawn Point Testing" )


		file.mapName = GetMapName().tolower()
	}
	else
	{






			player.ClientCommand( "unbind KP_RIGHTARROW" )
			player.ClientCommand( "unbind KP_LEFTARROW" )
			player.ClientCommand( "unbind KP_DOWNARROW" )
			player.ClientCommand( "unbind KP_5" )
			printt( "Disabled Spawn Point Testing" )


		file.spawnPointPosArray.clear()
	}
}































































































































































































#endif
