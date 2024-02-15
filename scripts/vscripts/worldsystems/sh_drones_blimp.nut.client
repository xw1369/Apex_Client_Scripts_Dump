global function ShBlimpDrones_EntitiesDidLoad
global function ShBlimpDrones_Init
global function ShBlimpDrones_GetScale
global function ShBlimpDrones_GetFlightSpeedMax
global function ShBlimpDrones_GetFlightAcceleration


global function BlimpDrones_SetBlimpDroneTrailFX
global function BlimpDrones_SetBlimpDroneTrailFXType
global function BlimpDrones_InitializeDroneLighting
global function BlimpDrones_InitializeDroneRUIs
global function BlimpDrones_DestroyDroneRUIs


global const asset BLIMP_DRONE_MODEL = $"mdl/celebration/celebration_blimp/celebration_blimp_01.rmdl"
global const float BLIMP_DRONE_ROLL = 5.0

global const string BLIMP_DRONE_LIVING_SOUND = "S20_Anniversary_Blimp_Mvmt_Flying"

global const string BLIMP_DRONE_MODEL_SCRIPTNAME = "BlimpDroneModel"
global const string BLIMP_DRONE_MOVER_SCRIPTNAME = "BlimpDroneMover"
global const string BLIMP_DRONE_ROTATOR_SCRIPTNAME = "BlimpDroneRotator"

global const string BLIMP_DRONE_NODE_SCRIPT_NAME = "blimp_drone_path_node"

const asset BLIMP_DRONE_FX_TRAIL = $"P_loot_drone_exhaust"
const asset BLIMP_DRONE_SCREEN_RUI = $"ui/td2_ad_01.rpak"

const float BLIMP_DRONE_FLIGHT_SPEED_MAX = 175.0
const float BLIMP_DRONE_FLIGHT_ACCEL = 100.0

const vector BLIMP_DRONE_LIGHTING_ORIGIN = <-2348, -3322, 1031.6>

const vector BLIMP_RIGHT_SCREEN_OFFSET = <120.458, 1481.042, 1395.37>
const vector BLIMP_RIGHT_SCREEN_ANGLES = <-16.65, 270.0, 0.0>
const vector BLIMP_LEFT_SCREEN_OFFSET = <120.458, -1481.042, 1395.37>
const vector BLIMP_LEFT_SCREEN_ANGLES = <196.65, 270.0, 180.0>
const float BLIMP_SCREEN_WIDTH = 2146.0
const float BLIMP_SCREEN_HEIGHT = 843.792
const float BLIMP_SCALE = 1.0

struct
{
	
	float flightSpeedMax						= 0.0
	float flightAcceleration					= 0.0

	
	vector lightingOrigin						= <0.0, 0.0, 0.0>
	vector rightScreenOffset					= <0.0, 0.0, 0.0>
	vector leftScreenOffset						= <0.0, 0.0, 0.0>
	float screenWidth							= 0.0
	float screenHeight							= 0.0
	float modelScale							= 0.0
} file


void function ShBlimpDrones_Init()
{
	file.flightSpeedMax														= GetCurrentPlaylistVarFloat( "blimp_drones_flight_speed_max", BLIMP_DRONE_FLIGHT_SPEED_MAX )
	file.flightAcceleration													= GetCurrentPlaylistVarFloat( "blimp_drones_flight_acceleration", BLIMP_DRONE_FLIGHT_ACCEL )

	file.modelScale															= GetCurrentPlaylistVarFloat( "blimp_drones_scale", BLIMP_SCALE )
	file.lightingOrigin														= StringToVector( GetCurrentPlaylistVarString( "blimp_drones_lighting_origin", format( "%.3f %.3f %.3f", BLIMP_DRONE_LIGHTING_ORIGIN.x, BLIMP_DRONE_LIGHTING_ORIGIN.y, BLIMP_DRONE_LIGHTING_ORIGIN.z ) ) )

	file.rightScreenOffset													= BLIMP_RIGHT_SCREEN_OFFSET * file.modelScale
	file.leftScreenOffset													= BLIMP_LEFT_SCREEN_OFFSET * file.modelScale
	file.screenWidth														= BLIMP_SCREEN_WIDTH * file.modelScale
	file.screenHeight														= BLIMP_SCREEN_HEIGHT * file.modelScale
}

void function ShBlimpDrones_EntitiesDidLoad()
{
	PrecacheModel( BLIMP_DRONE_MODEL )
	PrecacheParticleSystem( BLIMP_DRONE_FX_TRAIL )
}

float function ShBlimpDrones_GetScale()
{
	return file.modelScale
}

float function ShBlimpDrones_GetFlightSpeedMax()
{
	return file.flightSpeedMax
}

float function ShBlimpDrones_GetFlightAcceleration()
{
	return file.flightAcceleration
}


void function BlimpDrones_SetBlimpDroneTrailFX( DroneClientData droneData )
{
	entity droneEnt = droneData.model

	int fxId          = GetParticleSystemIndex( BLIMP_DRONE_FX_TRAIL )
	int attachIdx     = droneEnt.LookupAttachment( DEFAULT_DRONE_FX_ATTACH_NAME )
	int trailFXHandle = StartParticleEffectOnEntity( droneEnt, fxId, FX_PATTACH_POINT_FOLLOW, attachIdx )

	droneData.trailFXHandle = trailFXHandle
}

void function BlimpDrones_SetBlimpDroneTrailFXType( entity droneEnt, int trailType )
{
	if ( !ShDrones_IsValidDrone( droneEnt ) )
		return

	asset fxAsset
	int fxHandle
	DroneClientData clientData = ShDrones_GetDroneClientData( droneEnt )
	fxAsset = BLIMP_DRONE_FX_TRAIL
	fxHandle = clientData.trailFXHandle

	if ( EffectDoesExist( fxHandle ) )
		return

	int fxId = GetParticleSystemIndex( fxAsset )
	int attachIdx = droneEnt.LookupAttachment( ( DEFAULT_DRONE_FX_ATTACH_NAME ) )
	int trailFXHandle = StartParticleEffectOnEntity( droneEnt, fxId, FX_PATTACH_POINT_FOLLOW, attachIdx )

	clientData.trailFXHandle = trailFXHandle
}

void function BlimpDrones_InitializeDroneLighting( DroneClientData droneData )
{
	entity droneEnt = droneData.model

	if ( !IsValid( droneEnt ) )
		return

	droneEnt.SetLightingOrigin( file.lightingOrigin )
}

void function BlimpDrones_InitializeDroneRUIs( DroneClientData droneData )
{
	entity droneEnt = droneData.model

	if ( !IsValid( droneEnt ) )
		return

	
	{
		DroneRUIClientData rightScreenRUIClientData
		rightScreenRUIClientData.topology = CreateRUITopology_Worldspace( file.rightScreenOffset, BLIMP_RIGHT_SCREEN_ANGLES, file.screenWidth, file.screenHeight )
		rightScreenRUIClientData.rui  = RuiCreate( BLIMP_DRONE_SCREEN_RUI, rightScreenRUIClientData.topology, RUI_DRAW_WORLD, 0 )
		RuiTopology_SetParent( rightScreenRUIClientData.topology, droneEnt )
		droneData.droneRUIs.append( rightScreenRUIClientData )
	}

	
	{
		DroneRUIClientData leftScreenRUIClientData
		leftScreenRUIClientData.topology = CreateRUITopology_Worldspace( file.leftScreenOffset, BLIMP_LEFT_SCREEN_ANGLES, file.screenWidth, file.screenHeight )
		leftScreenRUIClientData.rui  = RuiCreate( BLIMP_DRONE_SCREEN_RUI, leftScreenRUIClientData.topology, RUI_DRAW_WORLD, 0 )
		RuiTopology_SetParent( leftScreenRUIClientData.topology, droneEnt )
		droneData.droneRUIs.append( leftScreenRUIClientData )
	}
}

void function BlimpDrones_DestroyDroneRUIs( DroneClientData droneData )
{
	foreach ( DroneRUIClientData ruiClientData in droneData.droneRUIs )
	{
		if ( ruiClientData.rui != null )
		{
			RuiDestroyIfAlive( ruiClientData.rui )
			ruiClientData.rui = null
		}

		if ( ruiClientData.topology != null )
		{
			RuiTopology_Destroy( ruiClientData.topology )
			ruiClientData.topology = null
		}
	}

	droneData.droneRUIs.clear()
}



