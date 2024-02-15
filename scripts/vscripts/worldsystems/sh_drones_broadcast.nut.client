global function ShBroadcastDrones_EntitiesDidLoad


global function BroadcastDrones_SetBroadcastDroneTrailFX
global function BroadcastDrones_SetBroadcastDroneTrailFXType


global const asset BROADCAST_DRONE_MODEL = $"mdl/td2/td2_drone_broadcasting_01.rmdl"
global const float BROADCAST_DRONE_ROLL = 0.1

const asset BROADCAST_DRONE_FX_TRAIL = $"P_td2_drone_wing_exhaust"

global const string BROADCAST_DRONE_LIVING_SOUND = "CameraDrone_Mvmt_Flying"

global const float BROADCAST_DRONE_MODEL_SCALE = 0.75
global const float BROADCAST_DRONE_FLIGHT_SPEED_MAX = 70.0
global const float BROADCAST_DRONE_FLIGHT_ACCEL = 1.0
global const float BROADCAST_DRONE_FLIGHT_SPEED_PANIC = 1.0
global const float BROADCAST_DRONE_PANIC_DURATION = 5.0

global const string BROADCAST_DRONE_MODEL_SCRIPTNAME = "BroadcastDroneModel"
global const string BROADCAST_DRONE_MOVER_SCRIPTNAME = "BroadcastDroneMover"
global const string BROADCAST_DRONE_ROTATOR_SCRIPTNAME = "BroadcastDroneRotator"

global const string BROADCAST_DRONE_NODE_SCRIPT_NAME = "broadcast_drone_path_node"

void function ShBroadcastDrones_EntitiesDidLoad()
{
	PrecacheModel( BROADCAST_DRONE_MODEL )
	PrecacheParticleSystem( BROADCAST_DRONE_FX_TRAIL )
}


void function BroadcastDrones_SetBroadcastDroneTrailFX( DroneClientData droneData )
{
	entity droneEnt = droneData.model

	int fxId          = GetParticleSystemIndex( BROADCAST_DRONE_FX_TRAIL )
	int attachIdx     = droneEnt.LookupAttachment( "fx_center" )
	int trailFXHandle = StartParticleEffectOnEntity( droneEnt, fxId, FX_PATTACH_POINT_FOLLOW, attachIdx )

	droneData.trailFXHandle = trailFXHandle
}

void function BroadcastDrones_SetBroadcastDroneTrailFXType( entity droneEnt, int trailType )
{
	if ( !ShDrones_IsValidDrone( droneEnt ) )
		return

	asset fxAsset
	int fxHandle
	DroneClientData clientData = ShDrones_GetDroneClientData( droneEnt )
	fxAsset = BROADCAST_DRONE_FX_TRAIL
	fxHandle = clientData.trailFXHandle

	if ( EffectDoesExist( fxHandle ) )
		return

	int fxId = GetParticleSystemIndex( fxAsset )
	int attachIdx = droneEnt.LookupAttachment( ( DEFAULT_DRONE_FX_ATTACH_NAME ) )
	int trailFXHandle = StartParticleEffectOnEntity( droneEnt, fxId, FX_PATTACH_POINT_FOLLOW, attachIdx )

	clientData.trailFXHandle = trailFXHandle
}

