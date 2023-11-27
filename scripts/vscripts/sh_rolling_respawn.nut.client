const string READY_TO_SPAWN = "#PL_READY_TO_SPAWN"
const asset RESPAWN_BEACON_MOBILE_MODEL = $"mdl/props/pathfinder_beacon_radar/pathfinder_beacon_radar_animated.rmdl"



struct {















} file





global function RollingRespawn_Init
global function RollingRespawn_RegisterNetworking













global function RollingRespawn_TriggerPlayerRespawn

global function ServerCallback_CL_CreateSpawnRegionRUI
global function ServerCallback_CL_PlayerReadyToSpawn
global function ServerCallback_CL_PlayerRespawned

global function ServerCallback_CL_ShowRespawnUI
global function ServerCallback_CL_HideRespawnUI





void function RollingRespawn_Init()
{

	if ( GetRespawnStyle() != eRespawnStyle.ROLLING_RESPAWN )
		return

	Remote_RegisterServerFunction( "ClientCallback_TryRespawnPlayer", "entity" )













		AddCallback_OnPingSpawnRequest( RollingRespawn_TriggerPlayerRespawn )
		AddCallback_OnCharacterSelectMenuOpened( Callback_HideRespawnOverlay )
		AddCallback_OnCharacterSelectMenuClosed( Callback_ShowRespawnOverlay )

}


void function RollingRespawn_RegisterNetworking()
{
	Remote_RegisterClientFunction( "ServerCallback_CL_CreateSpawnRegionRUI", "entity" )
	Remote_RegisterClientFunction( "ServerCallback_CL_PlayerReadyToSpawn" )
	Remote_RegisterClientFunction( "ServerCallback_CL_PlayerRespawned" )

	Remote_RegisterClientFunction( "ServerCallback_CL_ShowRespawnUI", "entity" )
	Remote_RegisterClientFunction( "ServerCallback_CL_HideRespawnUI", "entity" )
}




























































































































































































































































































void function Callback_HideRespawnOverlay()
{
	
}

void function Callback_ShowRespawnOverlay()
{
	
}

void function RollingRespawn_TriggerPlayerRespawn( entity player, entity waypoint )
{
	Remote_ServerCallFunction( "ClientCallback_TryRespawnPlayer", waypoint )
}

void function ServerCallback_CL_PlayerReadyToSpawn()
{
	AnnouncementMessageRight( GetLocalClientPlayer(), Localize( READY_TO_SPAWN ), "", < 182, 212, 209 >, $"", 7.0 )
}

void function ServerCallback_CL_PlayerRespawned()
{
	
}

void function ServerCallback_CL_CreateSpawnRegionRUI( entity spawnRegion )
{
	
}

void function ServerCallback_CL_ShowRespawnUI( entity waypoint )
{
	Waypoint_ShowOnLocalHud( waypoint )
}

void function ServerCallback_CL_HideRespawnUI( entity waypoint )
{
	Waypoint_HideOnLocalHud( waypoint )
}

