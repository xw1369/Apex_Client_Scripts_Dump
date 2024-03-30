
global function PathTT_Init
global function PathTT_OnNetworkRegistration
global function IsPathTTEnabled









global function ClInitPathTTRingTVEntities
global function SCB_PathTT_SetMessageIdxToCustomSpeakerIdx
global function SCB_PathTT_PlayRingAnnouncerDialogue
global function DEV_CheckIsPlayerInRing





enum ePathTTRingTVStates
{
	NO_PLAYERS = 0,
	ONE_SQUAD,
	MULTIPLE_SQUADS,
	RUN_AWAY,
	KNOCKOUT
}

const string FLAG_UPDATE_RING_TVS = "RingTVUpdate"
const float RING_TV_TEMP_MESSAGE_DISPLAY_TIME = 5.0
const float RING_TV_KNOCKOUT_TIME_ELAPSED_CAN_OVERRIDE = 4.0

const asset RING_CSV_DIALOGUE = $"datatable/dialogue/oly_path_tt_ring_announcer_dialogue.rpak"
const string BOXING_RING_MODEL = "mdl/test/davis_test/pathfinder_tt_ring_shield.rmdl"
global const string BOXING_RING_SCRIPTNAME = "pathfinder_tt_ring_shield"

const string FLAG_ARENA_LIGHTS_01 = "arena_lights_01"
const string FLAG_ARENA_LIGHTS_02 = "arena_lights_02"
const string FLAG_ARENA_LIGHTS_03 = "arena_lights_03"
const string FLAG_ARENA_LIGHTS_04 = "arena_lights_04"

const string FLAG_ARENA_TOP_GLOVES = "arena_top_gloves"
const string FLAG_ARENA_TOP_TEXT = "arena_top_text"

const string FLAG_ARENA_ROPES = "arena_ropes"

const string PLAYER_PASS_THROUGH_RING_SHIELD_SOUND = "Player_Enter_Ring_v1"
const string PLAYER_ENTER_RING_BELL = "Player_Enter_Ring_v2"





















struct
{




























	int   customQueueIdx
	int   currentlyPlayingLinePriority
	float announcerLineFinishedPlayingTime
	bool  isInStadium
	array<entity>	boxingRingCrowdAmbients_AudioPlaced
	


} file

void function PathTT_OnNetworkRegistration()
{
	Remote_RegisterClientFunction( "SCB_PathTT_SetMessageIdxToCustomSpeakerIdx", "int", 0, NUM_TOTAL_DIALOGUE_QUEUES )
	Remote_RegisterClientFunction( "SCB_PathTT_PlayRingAnnouncerDialogue", "int", 0, eRingAnnouncerLines._count )
	RegisterNetworkedVariable( "PathTT_IsCrowdActive", SNDC_GLOBAL, SNVT_BOOL, false )


		RegisterNetVarBoolChangeCallback( "PathTT_IsCrowdActive", OnIsCrowdActiveChanged )

}

void function PathTT_Init()
{
	AddCallback_EntitiesDidLoad( EntitiesDidLoad )






	ClInitPathTTRingTVEntities()

}

void function EntitiesDidLoad()
{
	if ( !IsPathTTEnabled() )
		return

	RegisterCSVDialogue( RING_CSV_DIALOGUE )
	PrecacheWeapon( $"mp_weapon_melee_boxing_ring" )
	PrecacheWeapon( $"melee_boxing_ring" )

	InitPathTTBoxingRing()





	InitPathTTBoxingRingEntities()
}

void function InitPathTTBoxingRing()
{















	AddCallback_OnWeaponStatusUpdate( Boxing_WeaponStatusCheck )


	PrecacheScriptString( BOXING_RING_SCRIPTNAME )

	FlagInit( FLAG_ARENA_LIGHTS_01 )
	FlagInit( FLAG_ARENA_LIGHTS_02 )
	FlagInit( FLAG_ARENA_LIGHTS_03 )
	FlagInit( FLAG_ARENA_LIGHTS_04 )
	FlagInit( FLAG_ARENA_TOP_GLOVES )
	FlagInit( FLAG_ARENA_TOP_TEXT )
	FlagInit( FLAG_ARENA_ROPES )
}








void function InitPathTTBoxingRingEntities()
{
	array<entity> enterTrigArr = GetEntArrayByScriptName( "path_tt_ring_trig" )
	if ( enterTrigArr.len() == 1 )
	{
		








			thread Cl_PathTT_MonitorIsPlayerInBoxingRing( enterTrigArr[ 0 ] )

	}
	else
	{
		Warning( "Warning! Couldn't find path TT enter trigger!" )
		return
	}


		array<entity> stadiumTrigArr = GetEntArrayByScriptName( "path_tt_stadium_trig" )
		if ( stadiumTrigArr.len() == 1 )
		{
			thread Cl_PathTT_MonitorIsPlayerInStadium( stadiumTrigArr[ 0 ] )
		}
		else
		{
			Warning( "Warning! Couldn't find client stadium trigger!" )
			return
		}

		array<entity> boxingRingAmbients_AudioPlaced = GetEntArrayByScriptName( "PathTT_Active_Crowd" )
		if ( boxingRingAmbients_AudioPlaced.len() == 0 )
		{
			Warning( "%s Warning! No audio-placed crowd ambients could be found for Path TT! Num found: %i", FUNC_NAME(), boxingRingAmbients_AudioPlaced.len() )
			return
		}
		foreach( entity ambient in boxingRingAmbients_AudioPlaced )
		{
			ambient.SetEnabled( false )
			file.boxingRingCrowdAmbients_AudioPlaced.append( ambient )
		}







































































}



































































const array<string> RING_ANNOUNCER_LINES = [
	"bc_OlyPathTTRing_recalibrate",
	"bc_OlyPathTTRing_run_away",
	"bc_OlyPathTTRing_enter_empty_ring",
	"bc_OlyPathTTRing_challenger",
	"bc_OlyPathTTRing_killed",
	"bc_OlyPathTTRing_downed",
	"bc_OlyPathTTRing_flawless_win",
	"bc_OlyPathTTRing_chain_kill"
]

const array<string> RING_ANNOUNCER_LINES_REVENANT = [
	"bc_OlyRevTTRing_recalibrate"
	"SR_OlyRevTTRing_runsAway"
	"SR_OlyRevTTRing_entersRing"
	"SR_OlyRevTTRing_challengeAccepted"
	"SR_OlyRevTTRing_killed"
	"SR_OlyRevTTRing_downed"
	"SR_OlyRevTTRing_winNoDmg"
	"SR_OlyRevTTRing_chainKill"

]
const array<string> RING_ANNOUNCER_LINES_REVENANT_EXT = [
	"bc_OlyRevTTRing_recalibrate_ext"
	"SR_OlyRevTTRing_runsAway_ext"
	"SR_OlyRevTTRing_entersRing_ext"
	"SR_OlyRevTTRing_challengeAccepted_ext"
	"SR_OlyRevTTRing_killed_ext"
	"SR_OlyRevTTRing_downed_ext"
	"SR_OlyRevTTRing_winNoDmg_ext"
	"SR_OlyRevTTRing_chainKill_ext"


]


const array<string> RING_ANNOUNCER_LINES_EXT  = [
	"bc_OlyPathTTRing_recalibrate_ext",
	"bc_OlyPathTTRing_run_away_ext",
	"bc_OlyPathTTRing_enter_empty_ring_ext",
	"bc_OlyPathTTRing_challenger_ext",
	"bc_OlyPathTTRing_killed_ext",
	"bc_OlyPathTTRing_downed_ext",
	"bc_OlyPathTTRing_flawless_win_ext",
	"bc_OlyPathTTRing_chain_kill_ext"
]







enum eRingAnnouncerLines
{
	recalibrating,
	run_away,
	enter_empty_ring,
	challenger,
	killed,
	downed,
	flawless_win,
	chain_kill,

	_count
}


void function SCB_PathTT_SetMessageIdxToCustomSpeakerIdx( int customQueueIdx )
{
	file.customQueueIdx = customQueueIdx
	RegisterCustomDialogueQueueSpeakerEntities( customQueueIdx, GetEntArrayByScriptName( "path_tt_announcer_speaker" ) )
}

const float ANNOUNCER_DEBOUNCE_TIME = 5.0
void function SCB_PathTT_PlayRingAnnouncerDialogue( int lineId )
{
	if ( Time() < file.announcerLineFinishedPlayingTime && ( file.currentlyPlayingLinePriority >= lineId ) )
	{
		return
	}

	string lineToPlay = file.isInStadium? RING_ANNOUNCER_LINES[ lineId ] : RING_ANNOUNCER_LINES_EXT[ lineId ]

		if ( IsNightMap() && UseNightRingAnnouncer() )
			lineToPlay = file.isInStadium? RING_ANNOUNCER_LINES_REVENANT[ lineId ] : RING_ANNOUNCER_LINES_REVENANT_EXT[ lineId ]

	float duration = GetSoundDuration( GetAnyDialogueAliasFromName( lineToPlay ) )
	file.announcerLineFinishedPlayingTime = Time() + duration + ANNOUNCER_DEBOUNCE_TIME
	file.currentlyPlayingLinePriority = lineId

	int dialogueFlags = eDialogueFlags.USE_CUSTOM_QUEUE | eDialogueFlags.USE_CUSTOM_SPEAKERS | eDialogueFlags.BLOCK_LOWER_PRIORITY_QUEUE_ITEMS
	PlayDialogueOnCustomSpeakers( GetAnyAliasIdForName( lineToPlay ), dialogueFlags, file.customQueueIdx )
}




void function ClInitPathTTRingTVEntities()
{
	ScreenOverrideInfo pathTTOverrideInfo
	pathTTOverrideInfo.scriptNameRequired = "path_tt_tv"
	pathTTOverrideInfo.skipStandardVars = true
	pathTTOverrideInfo.ruiAsset = $"ui/apex_screen_ptt.rpak"
	pathTTOverrideInfo.vars.float3s[ "customLogoSize" ] <- < 10, 10, 0 >
	pathTTOverrideInfo.bindEventIntA = true
	ClApexScreens_AddScreenOverride( pathTTOverrideInfo )
}






















































































































































































































































































































void function Cl_PathTT_MonitorIsPlayerInBoxingRing( entity trigger )
{
	entity player = GetLocalViewPlayer()
	if ( !IsValid( player ) )
		return

	EndSignal( player, "OnDestroy" )

	while ( true )
	{
		WaitSignal( trigger, "OnStartTouch", "OnEndTouch" )

		if ( IsAlive( player ) )
			PathTT_PlayerPassThroughRingShieldCeremony( player )
	}

}

void function Cl_PathTT_MonitorIsPlayerInStadium( entity trigger )
{
	entity player = GetLocalViewPlayer()
	if ( !IsValid( player ) )
		return

	EndSignal( player, "OnDestroy" )

	while ( true )
	{
		table signal = WaitSignal( trigger, "OnStartTouch", "OnEndTouch" )
		if ( signal.signal == "OnStartTouch" )
			file.isInStadium = true
		else
			file.isInStadium = false
	}
}

bool function DEV_CheckIsPlayerInRing()
{
	return file.isInStadium
}




























































































































void function PathTT_PlayerPassThroughRingShieldCeremony( entity player )
{
	vector org = player.GetOrigin()





		Signal( player, "DeployableBreachChargePlacement_End" )
		EmitSoundAtPosition( TEAM_UNASSIGNED, org, PLAYER_PASS_THROUGH_RING_SHIELD_SOUND )

}

enum ePathTTRingAudio
{
	CROWD,
	ANNOUNCER,

	_count
}






























void function OnIsCrowdActiveChanged( entity player, bool new )
{
	foreach( entity ambient in file.boxingRingCrowdAmbients_AudioPlaced )
	{
		ambient.SetEnabled( new )
	}
}



























































































































































































































































































































































































































































void function Boxing_WeaponStatusCheck( entity player, var rui, int slot )
{
	switch ( slot )
	{
		case OFFHAND_LEFT:
		case OFFHAND_INVENTORY:
			if ( StatusEffect_HasSeverity( player, eStatusEffect.is_boxing ) )
			{
				RuiSetBool( rui, "isBoxing", true )
			}
			else
			{
				RuiSetBool( rui, "isBoxing", false )
			}
			break
	}
}



bool function IsPathTTEnabled()
{
	if ( GetCurrentPlaylistVarBool( "path_tt_enabled", true ) )
	{
		return HasEntWithScriptName( "path_tt_jumbo_screen_ko" )
	}

	return false
}


