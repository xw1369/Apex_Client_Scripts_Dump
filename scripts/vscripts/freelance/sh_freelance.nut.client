
global function SharedInit_Freelance
global function Freelance_RegisterNetworking













global function SCB_UpdateTeamCanRespawn
global function Freelance_GetTeamCanRespawn
global function AddCallback_OnUpdateTeamCanRespawn
global function Freelance_GetWaitingToRespawnTextForState



global function GetTeamFromSquadID
global function GetSquadIDFromTeam









global enum eTeamCanRespawn
{
	YES,
	NO,
	NO_BECAUSE_IN_COMBAT,
	NO_BECAUSE_ENEMIES_NEARBY,
	NO_BECAUSE_BLEEDOUT_OUT,

	_count
}

const string WAYPOINTTYPE_SQUADHUDTRACKER = "SquadHudTrackerWP"

global const int MAX_SQUADS = 4


void function SharedInit_Freelance()
{
	PrecacheModel( $"mdl/dev/editor_ref.rmdl" ) 

	
	
	





	GamemodeSurvivalShared_Init()

	FreelanceNPCs_Init()

	SkitSystem_Init()
	ObjectiveSystem_Init()
	EncounterSystem_Init()







	OnscreenHints_Init()









	FreelanceMode_SharedInit()

	










	Waypoints_RegisterCustomType( WAYPOINTTYPE_SQUADHUDTRACKER, InstanceSquadHUDTrackerWP )

}


const string FUNCNAME_UpdateTeamCanRespawn = "SCB_UpdateTeamCanRespawn"

void function Freelance_RegisterNetworking()
{
	Remote_RegisterClientFunction( FUNCNAME_UpdateTeamCanRespawn, "int", 0, eTeamCanRespawn._count )

	NPCs_RegisterNetworking()
	FreelanceMode_RegisterNetworking()




}















int s_latestTeamCanRespawnState = eTeamCanRespawn.YES
void function SCB_UpdateTeamCanRespawn( int teamCanRespawnState )
{
	s_latestTeamCanRespawnState = teamCanRespawnState
	ExecuteCallbacks_OnUpdateTeamCanRespawnCallbacks( teamCanRespawnState )
}

int function Freelance_GetTeamCanRespawn()
{
	return s_latestTeamCanRespawnState
}

array< void functionref( int ) > s_onUpdateTeamCanRespawnCallbacks
void function AddCallback_OnUpdateTeamCanRespawn( void functionref( int ) func )
{
	s_onUpdateTeamCanRespawnCallbacks.append( func )
}

void function ExecuteCallbacks_OnUpdateTeamCanRespawnCallbacks( int teamCanRespawnState )
{
	foreach ( func in s_onUpdateTeamCanRespawnCallbacks )
		func( teamCanRespawnState )
}

string function Freelance_GetWaitingToRespawnTextForState( int teamCanRespawn )
{
	switch ( teamCanRespawn )
	{
		case eTeamCanRespawn.NO_BECAUSE_IN_COMBAT:
			return "Waiting to Respawn, Teammate In Combat..."
		case eTeamCanRespawn.NO_BECAUSE_ENEMIES_NEARBY:
			return "Waiting to Respawn, Enemies Nearby..."
		case eTeamCanRespawn.NO_BECAUSE_BLEEDOUT_OUT:
			return "Waiting to Respawn, Teammate Bleeding Out..."
	}

	return "Waiting to Respawn..."
}







int function GetTeamFromSquadID( int squad )
{
	Assert( (squad >= 0) && (squad < MAX_SQUADS) )
	int teamNum = (TEAM_MULTITEAM_FIRST + squad)
	return teamNum
}

int function GetSquadIDFromTeam( int team )
{
	int squad = (team - TEAM_MULTITEAM_FIRST)
	Assert( (squad >= 0) && (squad < MAX_SQUADS) )
	return squad
}







































void function InstanceSquadHUDTrackerWP_OnDestroyed( entity wp )
{
	printf( "%s(): %s", FUNC_NAME(), string( wp ) )
	if ( wp.wp.ruiHud != null )
		RuiSetBool( wp.wp.ruiHud, "isFinished", true )
}

void function InstanceSquadHUDTrackerWP_( entity wp )
{
	printf( "%s(): %s", FUNC_NAME(), string( wp ) )
	var rui = CreateWaypointRui( $"ui/squad_hud_tracker.rpak", 400 )

	RuiSetGameTime( rui, "introStartTime", wp.GetWaypointGametime( 0 ) )

	
	entity player = GetLocalViewPlayer()
	RuiSetInt( rui, "localSquadIndex", player.GetSquadID() )

	
	RuiTrackFloat( rui, "progressA", wp, RUI_TRACK_WAYPOINT_FLOAT, 0 )
	RuiTrackFloat( rui, "progressB", wp, RUI_TRACK_WAYPOINT_FLOAT, 1 )
	RuiTrackFloat( rui, "progressC", wp, RUI_TRACK_WAYPOINT_FLOAT, 2 )
	RuiTrackFloat( rui, "progressD", wp, RUI_TRACK_WAYPOINT_FLOAT, 3 )

	Assert( wp.wp.ruiHud == null )
	SetWaypointRui_HUD( wp, rui )
	AddEntityDestroyedCallback( wp, InstanceSquadHUDTrackerWP_OnDestroyed )
}

void function InstanceSquadHUDTrackerWP( entity wp )
{
	InstanceSquadHUDTrackerWP_( wp )
}



















































































































