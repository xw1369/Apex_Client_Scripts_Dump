global function CryptoTT_Init
global function InitCryptoMap
global function CryptoTT_OnNetworkRegistration
global function IsCryptoTTEnabled













global function ClCryptoTVsInit
global function SCB_DisplayEnemiesOnMinimap_CryptoTT
global function SCB_PlayFullAndMinimapSequence_CryptoTT
global function SCB_ShowSatelliteChargeRUI
global function RegisterCLCryptoCallbacks


const float CRYPTO_MAP_WORLD_SCAN_SCALE = 80000
const float CRYPTO_MAP_SCAN_NOTIFICATION_DIST = 12500
const float CRYPTO_MAP_PROJECTION_RADIUS = 352
const float CRYPTO_MAP_PROJECTION_RADIUS_BUFFER = 16
const float CRYPTO_WORLD_TO_HOLO_SCALE = 0.00685
const vector CRYPTO_HOLO_OFFSET = < -32, 32, 16 >
const float CRYPTO_TT_TRIGGER_RADIUS = 1536
const float CRYPTO_TT_HOLO_MAP_RUI_INTERACT_RADIUS = 576
const float CRYPTO_TT_TRIGGER_EXIT_RADIUS = CRYPTO_TT_TRIGGER_RADIUS + 256.0

const float CRYPTO_TT_BUTTON_USE_TIME = 5.0

const float CRYPTO_TT_MAP_SCAN_DELAY = 45.0
const float CRYPTO_TT_PREFIRE_TIME = 38.0/30

const float CRYPTO_TT_ENEMY_MINIMAP_ICON_TIME_BEFORE_FADE = 10.0
const float CRYPTO_TT_ENEMY_MINIMAP_ICON_FADE_TIME = 10.0
const float CRYPTO_TT_TINT_DURATION = CRYPTO_TT_ENEMY_MINIMAP_ICON_TIME_BEFORE_FADE + CRYPTO_TT_ENEMY_MINIMAP_ICON_FADE_TIME

const string CRYPTO_TT_MAP_MOVER_SCRIPTNAME = "crypto_tt_map_mover"































































const float CRYPTO_MAP_TOPO_ICON_WIDTH = 10.0
const float CRYPTO_MAP_TOPO_WIDTH = 178.0
const float CRYPTO_MAP_TOPO_HEIGHT = 128.0
const float CRYPTO_MAP_TOPO_FLYOUT_WIDTH = 8.0

const float CRYPTO_MAP_RUI_ORIENT_MIN_DIST = 8.0
const float CRYPTO_MAP_RUI_LOOKING_AT_ORIENT_MIN_DIST = 16.0	
const float CRYPTO_MAP_RUI_EXPANDED_LOOKING_AT_ORIENT_MIN_DIST = 48.0	
const vector CRYPTO_MAP_RUI_OFFSET = < CRYPTO_MAP_TOPO_ICON_WIDTH + 8.0, 64, 0 >
const vector CRYPTO_MAP_RUI_EXPANDED_OFFSET = < 80, 64, 0 >

const float CRYPTO_MAP_FOCUS_MIN_DISTANCE = 80.0
const float CRYPTO_MAP_FOCUS_BLOCK_RADIUS = 5.0
const float CRYPTO_MAP_FOCUS_ENTER_RADIUS = 10.0
const float CRYPTO_MAP_FOCUS_EXIT_RADIUS = 64.0

const float CRYPTO_MAP_FOCUS_TIME_TO_ACTIVATE = 1.0
const float CRYPTO_ACTIVATED_PANEL_INACTIVE_HIDE_TIME = 7.5

const vector CRYPTO_MAP_FOCUS_ENTER_OFFSET = < 0, 10, 0 >

enum eCryptoRUIColorIdxs
{
	NEUTRAL = 1,
	LOCKED,
	U_R_HERE

}






























struct HoloMapRUIData
{
	var    	topo
	var    	rui
	var    	topo_FlyoutLine
	var		rui_FlyoutLine
	bool   	canExpand
	string 	hatchId
	vector 	origin
	vector 	originExpanded
	vector	originFlyout
	bool   	isExpanded
}


struct
{
	vector           	cryptoHoloMapOrigin
	vector           	cryptoHoloProjBasinOrigin
	entity           	cryptoHoloMapEnt

	float				mapScanCooldown = CRYPTO_TT_MAP_SCAN_DELAY


















		bool playerInHoloRoom
		bool allCryptoRuisHidden
		array<ApexScreenState> cryptoTTApexScreenStates
		array< HoloMapRUIData > allHoloMapRUIData
		array< HoloMapRUIData > expandableHoloMapRUIData
		HoloMapRUIData& holoMap_YouAreHere

} file

void function CryptoTT_OnNetworkRegistration()
{
	Remote_RegisterClientFunction( "SCB_DisplayEnemiesOnMinimap_CryptoTT", "float", 0.0, 9999.9, 32 )
	Remote_RegisterClientFunction( "SCB_PlayFullAndMinimapSequence_CryptoTT", "bool", "float", 0.0, 9999.9, 32 )
	Remote_RegisterClientFunction( "SCB_ShowSatelliteChargeRUI", "float", 0.0, 9999.9, 32 )
}


void function RegisterCLCryptoCallbacks()
{

	AddCreateCallback( "trigger_cylinder", OnCryptoTTHoloMapRoomTriggerCreated ) 
}


void function CryptoTT_Init()
{
	AddCallback_EntitiesDidLoad( EntitiesDidLoad )


		ClCryptoTVsInit()
		RegisterCLCryptoCallbacks()

}










































































































































void function InitCryptoMap()
{







	
	array<entity> cryptoHoloMapEnt_Raw = GetEntArrayByScriptName( "crypto_tt_holo_map_center" )
	if ( cryptoHoloMapEnt_Raw.len() != 1 )
	{
		Warning( "!!! Warning !!! Missing ent for crypto holo map " + cryptoHoloMapEnt_Raw.len() )
		foreach( entity mapEnt in cryptoHoloMapEnt_Raw )
		{
			DebugDrawSphere( mapEnt.GetOrigin(), 4, COLOR_RED, true, 10.0 )
			printt( "!!! Entity:", mapEnt )
		}
		return
	}

	array<entity> cryptoSwitch_Raw = GetEntArrayByScriptName( "crypto_map_switch" )
	if ( cryptoSwitch_Raw.len() != 1 )
	{
		Warning( "!!! Warning !!! Incorrect number of crypto map switches! " + cryptoSwitch_Raw.len() )
		return
	}

	
	file.mapScanCooldown = GetCurrentPlaylistVarFloat( "crypto_map_scan_cooldown_override", CRYPTO_TT_MAP_SCAN_DELAY )













	file.cryptoHoloMapEnt 				= cryptoHoloMapEnt_Raw[ 0 ]
	file.cryptoHoloProjBasinOrigin 		= file.cryptoHoloMapEnt.GetOrigin()
	file.cryptoHoloMapOrigin 			= file.cryptoHoloProjBasinOrigin
	file.cryptoHoloMapOrigin 			+= LocalDirToWorldDir( CRYPTO_HOLO_OFFSET, file.cryptoHoloMapEnt )












		
		AddCallback_ItemFlavorLoadoutSlotDidChange_AnyPlayer( Loadout_Character(), CryptoTT_OnPlayerChangeLoadout )

		if ( IsCanyonlandsBunkersEnabled() )
		{
			foreach ( string id in HATCH_ZONE_IDS )
			{
				entity ruiTarget
				bool isQuestHatch = id == "12_treasure"
				if ( isQuestHatch )
					ruiTarget = GetEntByScriptName( format( HATCH_DOOR_LEAVE_SCRIPTNAME, id ) )
				else
					ruiTarget = GetEntByScriptName( format( HATCH_DOOR_ENTRANCE_SCRIPTNAME, id ) )

				vector origin          = WorldToCryptoMapPos( ruiTarget.GetOrigin() ) + < 0, 0, 64 >
				bool bunkerIsUnlocked  = IsHatchBunkerUnlocked( id )
				HoloMapRUIData ruiData = CryptoTT_CreateAndRegisterHoloMapRUIData( origin, CryptoTT_GetIconAssetForBunker( id ), CryptoTT_GetColorIdxForBunker( id ), !isQuestHatch && bunkerIsUnlocked )
				RuiSetBool( ruiData.rui, "isLocked", !bunkerIsUnlocked )
				ruiData.hatchId = id
				CryptoTT_UpdateHoloMapRUIText( ruiData, id, false )
			}
		}

		vector uRHereOrigin = WorldToCryptoMapPos( file.cryptoHoloMapOrigin ) + < 0, 0, 64 >
		file.holoMap_YouAreHere = CryptoTT_CreateAndRegisterHoloMapRUIData( uRHereOrigin, GetAssetFromString( "rui/hud/crypto_tt_holo_map/icon_crypto_tt_holomap_u_r_here" ), eCryptoRUIColorIdxs.U_R_HERE, false )
		RuiSetFloat( file.holoMap_YouAreHere.rui, "unfocusedOpacity", 0.75 )
		RuiSetString( file.holoMap_YouAreHere.rui, "collapsedText", "" )
		CryptoTT_HoloMap_OrientHoloRuis( GetLocalViewPlayer() )


	entity cryptoTTSwitch = cryptoSwitch_Raw[ 0 ]

	if ( GetCurrentPlaylistVarBool( "crypto_map_scan_enabled", true ) )
		CryptoTT_HoloMap_SetButtonUsable( cryptoTTSwitch )






	if ( GetCurrentPlaylistVarBool( "crypto_map_scan_enabled", true ) )
		AddCreateCallback( "prop_dynamic", HoloMapPodiumSpawned )


























}

void function EntitiesDidLoad()
{
	if ( !IsCryptoTTEnabled() )
		return

	PrecacheScriptString( CRYPTO_TT_MAP_MOVER_SCRIPTNAME )










	InitCryptoMap()




}

void function HoloMapPodiumSpawned( entity panel )
{

	if( panel.GetScriptName() != "crypto_map_switch")
		return

	CryptoTT_HoloMap_SetButtonUsable(panel)
}


void function OnCryptoTTHoloMapRoomTriggerCreated( entity trigger )
{
	if ( trigger.GetTargetName() != "ctt_holo_room_trig" )
		return

	trigger.SetClientEnterCallback( CryptoTT_PlayerEnterRoomTrig )
}


void function CryptoTT_HoloMap_SetButtonUsable( entity prop )
{




	AddCallback_OnUseEntity_ClientServer( prop, HoloMap_OnUse )
	SetCallback_CanUseEntityCallback( prop, HoloMap_CanUse )
}

bool function HoloMap_CanUse( entity user, entity button, int useFlags )
{
	if ( Bleedout_IsBleedingOut( user ) )
		return false

	if ( user.ContextAction_IsActive() )
		return false

	entity activeWeapon = user.GetActiveWeapon( eActiveInventorySlot.mainHand )
	if ( IsValid( activeWeapon ) && activeWeapon.IsWeaponOffhand() )
		return false

	if ( button.e.isBusy )
		return false


	return true
}















void function HoloMap_OnUse( entity panel, entity user, int useInputFlags )
{
	if ( !IsBitFlagSet( useInputFlags, USE_INPUT_LONG ) )
		return

	ExtendedUseSettings settings

	settings.duration = CRYPTO_TT_BUTTON_USE_TIME
	settings.useInputFlag = IN_USE_LONG
	settings.successSound = "lootVault_Access"


		settings.loopSound = "survival_titan_linking_loop"
		settings.displayRuiFunc = DisplayHoldUseRUIForCryptoTTSatellite
		settings.displayRui = $"ui/health_use_progress.rpak"
		settings.icon = $"rui/hud/gametype_icons/survival/data_knife"
		settings.hint = Localize( "#HINT_SAT_ACTIVATE" )



	settings.successFunc = CryptoTTScan_UseSuccess












	thread ExtendedUse( panel, user, settings )
}


void function DisplayHoldUseRUIForCryptoTTSatellite( entity ent, entity player, var rui, ExtendedUseSettings settings )
{
	DisplayHoldUseRUIForCryptoTTSatellite_Internal( rui, settings.icon, Time(), Time() + settings.duration, settings.hint )
}

void function DisplayHoldUseRUIForCryptoTTSatellite_Internal( var rui, asset icon, float startTime, float endTime, string hint )
{
	RuiSetBool( rui, "isVisible", true )
	RuiSetImage( rui, "icon", icon )
	RuiSetGameTime( rui, "startTime", startTime )
	RuiSetGameTime( rui, "endTime", endTime )
	RuiSetString( rui, "hintKeyboardMouse", hint )
	RuiSetString( rui, "hintController", hint )
}











































































































































































void function CryptoTTScan_UseSuccess( entity button, entity player, ExtendedUseSettings settings )
{



}


const float CRYPTO_TT_CHARGE_RUI_WAIT_BEFORE_REVEAL = 5.0
void function SCB_ShowSatelliteChargeRUI( float useSuccessTime )
{
	thread SCB_ShowSatelliteChargeRUI_Thread( useSuccessTime )
}

void function SCB_ShowSatelliteChargeRUI_Thread( float useSuccessTime )
{
	
	if ( !IsValid( file.holoMap_YouAreHere.rui ) || ( file.holoMap_YouAreHere.rui == null ) )
		return

	float visualizedChargeTime = file.mapScanCooldown + CRYPTO_TT_PREFIRE_TIME - CRYPTO_TT_CHARGE_RUI_WAIT_BEFORE_REVEAL
	float startTime = useSuccessTime + CRYPTO_TT_CHARGE_RUI_WAIT_BEFORE_REVEAL
	float endTime = useSuccessTime + file.mapScanCooldown + CRYPTO_TT_PREFIRE_TIME
	while ( Time() < endTime )
	{
		if ( Time() > startTime )
		{
			float percentProgress = ( Time() - startTime ) / visualizedChargeTime
			percentProgress = min( 1.0, max( 0.0, percentProgress ) )
			int progressForText = int( percentProgress * 100 )
			RuiSetString( file.holoMap_YouAreHere.rui, "collapsedText", Localize( "#CTT_SAT_CHARGE_PERCENT_SIGN", progressForText ) )
		}

		WaitFrame()
	}

	RuiSetString( file.holoMap_YouAreHere.rui, "collapsedText", Localize( "#CTT_SAT_CHARGE_PERCENT_SIGN", 100 ) )
	wait 2.0

	RuiSetString( file.holoMap_YouAreHere.rui, "collapsedText", "" )
}






































































































































































































































































































































































































































































































































vector function WorldToCryptoMapPos( vector origin )
{
	entity holoMapEnt = file.cryptoHoloMapEnt
	vector mappedPoint = ( origin.x * holoMapEnt.GetRightVector() ) + ( origin.y * holoMapEnt.GetForwardVector() ) + ( origin.z * holoMapEnt.GetUpVector() )
	mappedPoint *= CRYPTO_WORLD_TO_HOLO_SCALE
	return mappedPoint + file.cryptoHoloMapOrigin
}



void function ClCryptoTVsInit()
{
	ScreenOverrideInfo cryptoTTOverrideInfo
	cryptoTTOverrideInfo.scriptNameRequired = "ctt_tv"
	cryptoTTOverrideInfo.skipStandardVars = true
	cryptoTTOverrideInfo.ruiAsset = $"ui/apex_screen_ctt.rpak"
	cryptoTTOverrideInfo.vars.float3s[ "customLogoSize" ] <- < 10, 10, 0 >
	cryptoTTOverrideInfo.vars.images[ "logo" ] <- $"rui/hud/common/crypto_logo"
	cryptoTTOverrideInfo.bindEventIntA = true
	ClApexScreens_AddScreenOverride( cryptoTTOverrideInfo )
}

void function SCB_PlayFullAndMinimapSequence_CryptoTT( bool shouldTintMap, float tintDuration )
{
	FullMap_PlayCryptoPulseSequence( file.cryptoHoloMapOrigin, shouldTintMap, tintDuration )
}

void function SCB_DisplayEnemiesOnMinimap_CryptoTT( float endTime )
{
	thread CryptoTT_DisplayEnemiesOnMinimap_Thread( endTime )
}

void function CryptoTT_DisplayEnemiesOnMinimap_Thread( float endTime )
{
	entity player = GetLocalViewPlayer()
	if ( !IsValid( player ) )
		return

	int team = player.GetTeam()
	array<entity> aliveEnemies = GetPlayerArrayOfEnemies_Alive( team )

	float timeToWait = endTime - Time()
	float timeToStartFade = Time() + CRYPTO_TT_ENEMY_MINIMAP_ICON_TIME_BEFORE_FADE
	float timeToEndFade = timeToStartFade + CRYPTO_TT_ENEMY_MINIMAP_ICON_FADE_TIME

	array<var> fullMapRuis
	array<var> minimapRuis
	array<entity> entsForTracking
	foreach( entity enemy in aliveEnemies )
	{
		
		var fRui = FullMap_AddEnemyLocation( enemy )
		
		fullMapRuis.append( fRui )

		var mRui = Minimap_AddEnemyToMinimap( enemy )
		minimapRuis.append( mRui )
		RuiSetGameTime( mRui, "fadeStartTime", timeToStartFade )
		RuiSetGameTime( mRui, "fadeEndTime", timeToEndFade )
	}

	EmitSoundOnEntity( player, "Canyonlands_Scr_Cryto_TT_Allies_Enemies_Revealed" )

	if ( timeToWait > 0 )
	{
		
		float testIsThereTimeUntilStartFade = timeToWait - CRYPTO_TT_ENEMY_MINIMAP_ICON_TIME_BEFORE_FADE
		if ( testIsThereTimeUntilStartFade > 0 )
		{
			timeToWait -= CRYPTO_TT_ENEMY_MINIMAP_ICON_TIME_BEFORE_FADE
			wait CRYPTO_TT_ENEMY_MINIMAP_ICON_TIME_BEFORE_FADE
			foreach( var fRui in fullMapRuis )
			{
				RuiSetGameTime( fRui, "fadeOutEndTime", timeToEndFade )
			}

		}
		wait timeToWait
	}

	foreach( var ruiToDestroy in fullMapRuis )
	{
		Fullmap_RemoveRui( ruiToDestroy )
		RuiDestroy( ruiToDestroy )
	}

	foreach( var ruiToDestroy in minimapRuis)
	{
		Minimap_CommonCleanup( ruiToDestroy )
	}
}

void function CryptoTT_PlayerEnterRoomTrig( entity trigger, entity player )
{
	
	if( !player.IsPlayer() )
		return

	if ( !file.playerInHoloRoom )
	{
		CryptoTT_SetPlayerInHoloRoom( true )
		thread CryptoTT_MonitorShouldUpdateRUIForPlayer( trigger, player )
		thread CryptoTT_HoloMap_OrientRuiThink( trigger, player )
		thread CryptoTT_HoloMap_ExpandRUIThink( trigger, player )
	}

	if ( file.allCryptoRuisHidden )
	{
		CryptoTT_SetAllRUIHidden( false )
	}
}

void function CryptoTT_SetPlayerInHoloRoom( bool isInHoloRoom )
{
	file.playerInHoloRoom = isInHoloRoom
}


void function CryptoTT_MonitorShouldUpdateRUIForPlayer( entity trigger, entity player )
{
	EndSignal( player, "OnDestroy" )
	EndSignal( player, "OnDeath" )

	
	OnThreadEnd(
		function() : ( player )
		{
			if ( !IsAlive( player ) )
			{
				CryptoTT_SetAllRUIHidden( true )
				CryptoTT_SetPlayerInHoloRoom( false )
			}
		}
	)

	
	while ( file.playerInHoloRoom )
	{
		
		vector playerOrg = player.GetOrigin()
		float dist2D = Distance2D( trigger.GetOrigin(), playerOrg )
		if ( dist2D > CRYPTO_TT_TRIGGER_EXIT_RADIUS )
			CryptoTT_SetPlayerInHoloRoom( false )

		WaitFrame()
	}
}

void function CryptoTT_SetAllRUIHidden( bool shouldHide )
{
	file.allCryptoRuisHidden = shouldHide
	foreach( HoloMapRUIData ruiData in file.allHoloMapRUIData )
	{
		RuiSetBool( ruiData.rui, "forceHide", shouldHide )
		if ( ruiData.canExpand )
			RuiSetBool( ruiData.rui_FlyoutLine, "forceHide", shouldHide )
	}
}

void function CryptoTT_HoloMap_OrientRuiThink( entity trigger, entity player )
{
	EndSignal( player, "OnDestroy" )
	EndSignal( player, "OnDeath" )

	while ( file.playerInHoloRoom )
	{
		
		CryptoTT_HoloMap_OrientHoloRuis( player )

		WaitFrame()
	}
}

void function CryptoTT_HoloMap_OrientHoloRuis( entity player )
{
	foreach( HoloMapRUIData ruiData in file.allHoloMapRUIData )
	{
		vector forward     = player.CameraPosition() - ruiData.origin
		bool playerLookingAtRUI = DotProduct( AnglesToForward( player.CameraAngles() ), forward ) < 0
		vector forwardNorm = Normalize( < forward.x, forward.y, 0 > )
		vector rightDir    = CrossProduct( forwardNorm, < 0, 0, 1 > )

		
		float rightOffset = CRYPTO_MAP_RUI_OFFSET.x
		if ( ruiData.isExpanded )
		{
			rightOffset = CRYPTO_MAP_RUI_EXPANDED_OFFSET.x

			rightOffset = ( CRYPTO_MAP_TOPO_WIDTH * 0.5 ) + 10
			forward = player.CameraPosition() - ruiData.originExpanded
			forwardNorm = Normalize( < forward.x, forward.y, 0 > )
			rightDir = CrossProduct( forwardNorm, < 0, 0, 1 > )

			
			{
				vector expandedRuiLeftCorner  = ruiData.originExpanded - (rightDir * ((rightOffset * 0.5) + 16))
				vector flyoutTopoBottomCorner = ruiData.origin - < 0, 0, CRYPTO_MAP_TOPO_HEIGHT * 0.5 >
				vector cornerToCorner         = expandedRuiLeftCorner - flyoutTopoBottomCorner
				vector cornerToCornerMidpoint = (expandedRuiLeftCorner + flyoutTopoBottomCorner) * 0.5

				vector flyoutDown             = Normalize( cornerToCorner )
				vector flyoutForwardNorm      = Normalize( FlattenVec( player.CameraPosition() - cornerToCornerMidpoint ) )
				vector flyoutRight            = Normalize( CrossProduct( flyoutDown, flyoutForwardNorm ) )

				RuiTopology_UpdatePos( ruiData.topo_FlyoutLine, flyoutTopoBottomCorner - (flyoutRight * 4), flyoutRight * CRYPTO_MAP_TOPO_FLYOUT_WIDTH, flyoutDown * (Length( cornerToCorner ) + 4) )
			}

		}

		float distToRUI = Length2D( forward )
		float minDist   = CRYPTO_MAP_RUI_LOOKING_AT_ORIENT_MIN_DIST
		if ( ruiData.isExpanded )
			minDist = CRYPTO_MAP_RUI_EXPANDED_LOOKING_AT_ORIENT_MIN_DIST

		
		bool shouldSkipOrientation = ( distToRUI < CRYPTO_MAP_RUI_ORIENT_MIN_DIST ) || ( playerLookingAtRUI && ( distToRUI < minDist ) )
		if ( shouldSkipOrientation )
			continue

		float rightWidth   = CRYPTO_MAP_TOPO_WIDTH
		vector down        = CrossProduct( forwardNorm, rightDir )
		float downHeight   = CRYPTO_MAP_TOPO_HEIGHT

		RuiTopology_UpdatePos( ruiData.topo,  CryptoTT_GetRuiOriginToUse( ruiData ) - ( rightDir * rightOffset ) - ( down * CRYPTO_MAP_RUI_OFFSET.y ), rightDir * rightWidth, down * downHeight )
	}
}

const float CRYPTO_TT_VIS_TRACE_INTERVAL = 0.1
const float CAN_EXPAND_RADIUS_FROM_MAP = 640
const float CAN_EXPAND_RADIUS_FROM_MAP_SQR = CAN_EXPAND_RADIUS_FROM_MAP * CAN_EXPAND_RADIUS_FROM_MAP

void function CryptoTT_HoloMap_ExpandRUIThink( entity trigger, entity player, bool debug = false)
{
	EndSignal( player, "OnDestroy" )
	EndSignal( player, "OnDeath" )

	HoloMapRUIData activatedPanel
	HoloMapRUIData focusCandidate
	HoloMapRUIData prevFocusCandidate
	float focusCandidateStartTime
	float activatedFocusCandidateStartTime
	float traceFrac
	float lastTraceTime
	while ( file.playerInHoloRoom )
	{
		float playerDistToMapSqr = Distance2DSqr( player.GetOrigin(), file.cryptoHoloMapOrigin )
		bool playerCloseEnoughToExpandRUI = playerDistToMapSqr <= CAN_EXPAND_RADIUS_FROM_MAP_SQR

		vector playerCamOrg 		= player.CameraPosition()
		bool closestAngleOverridden = false
		float closestAngle 			= 90.0
		float closestDistance 		= 10000000
		bool closestDistIsBlocking 	= false
		int closestAngleIdx 		= -1
		int closestDistanceIdx 		= -1
		array<HoloMapRUIData> focusCandidates
		int numRuis = file.expandableHoloMapRUIData.len()

		int testVisColGroup = TRACE_COLLISION_GROUP_NONE
		int testVisCollMask = (TRACE_MASK_VISIBLE_AND_NPCS | CONTENTS_BLOCKLOS | CONTENTS_BLOCK_PING | CONTENTS_HITBOX)

		
		if ( playerCloseEnoughToExpandRUI && ( ( Time() - lastTraceTime ) > CRYPTO_TT_VIS_TRACE_INTERVAL ) )
		{
			vector orgStart = playerCamOrg
			vector orgEnd = playerCamOrg + ( AnglesToForward( player.CameraAngles() ) * CRYPTO_TT_TRIGGER_EXIT_RADIUS )
			TraceResults tr = TraceLine( orgStart, orgEnd, [player], testVisCollMask, testVisColGroup )
			traceFrac = tr.fraction
			vector hitPos = orgStart + ( orgEnd - orgStart ) * tr.fraction
			lastTraceTime = Time()
		}

		
		if ( playerCloseEnoughToExpandRUI )
		{
			for( int i; i < numRuis; i++ )
			{
				HoloMapRUIData ruiData = file.expandableHoloMapRUIData[ i ]
				bool isActivatedPanel = ruiData.topo == activatedPanel.topo

				
				vector viewTargetOrigin = CryptoTT_GetRuiOriginToUse( ruiData )
				vector offsetUpDir      = < 0, 0, 1 >
				vector offsetFwdDir     = Normalize( viewTargetOrigin - playerCamOrg )
				vector offsetRightDir   = CrossProduct( offsetFwdDir, offsetUpDir )
				vector offset           = CRYPTO_MAP_FOCUS_ENTER_OFFSET

				viewTargetOrigin += ( offsetRightDir * offset.x ) + ( offsetUpDir * offset.y )

				float distToTarget = Distance( viewTargetOrigin, playerCamOrg )
				if ( distToTarget < CRYPTO_MAP_FOCUS_MIN_DISTANCE )
					continue

				
				float traceDist = traceFrac * CRYPTO_TT_TRIGGER_EXIT_RADIUS
				if ( distToTarget > traceDist )
					continue

				
				float radiusTarget = isActivatedPanel ? CRYPTO_MAP_FOCUS_EXIT_RADIUS : CRYPTO_MAP_FOCUS_ENTER_RADIUS
				float fovTarget = asin( radiusTarget / distToTarget ) * RAD_TO_DEG
				float fovBlockTarget = asin( CRYPTO_MAP_FOCUS_BLOCK_RADIUS / distToTarget ) * RAD_TO_DEG

				if ( debug )
				{
					vector debugColor = COLOR_RED
					if ( isActivatedPanel )
						debugColor = COLOR_GREEN
					DebugDrawFOVCircle( viewTargetOrigin, player.CameraPosition(), fovTarget, debugColor, true, 0.1 )
					DebugDrawFOVCircle( viewTargetOrigin, player.CameraPosition(), fovBlockTarget, COLOR_YELLOW, true, 0.1 )
					if ( isActivatedPanel )
						DebugDrawSphere( ruiData.originExpanded, 4, COLOR_WHITE, true, 0.1 )
				}

				
				vector playerToRuiDir = Normalize( viewTargetOrigin - playerCamOrg )
				float facingDot = DotProduct( playerToRuiDir, AnglesToForward( player.CameraAngles() ) )
				float dotAngle = DotToAngle( facingDot )

				if ( dotAngle < fovTarget )
				{
					
					
					if ( isActivatedPanel || ( ( dotAngle < closestAngle ) && !closestAngleOverridden ) )
					{
						closestAngle = dotAngle
						closestAngleIdx = i
						if ( isActivatedPanel )
						{
							closestAngleOverridden = true
						}
					}

					float dist2D = Distance2D( playerCamOrg, viewTargetOrigin )
					if ( dist2D < closestDistance )
					{
						closestDistance = dist2D
						closestDistanceIdx = i
						closestDistIsBlocking = dotAngle < fovBlockTarget
					}
				}
			}
		}

		if ( closestAngleIdx >= 0 && closestDistanceIdx >= 0 )
		{
			prevFocusCandidate = focusCandidate
			
			if ( closestAngleIdx != closestDistanceIdx )
			{
				float closestAngleDist = Distance2D( file.expandableHoloMapRUIData[ closestAngleIdx ].origin, playerCamOrg )
				if ( closestDistIsBlocking )
					focusCandidate = file.expandableHoloMapRUIData[ closestDistanceIdx ]
				else
					focusCandidate = file.expandableHoloMapRUIData[ closestAngleIdx ]
			}
			else
				focusCandidate = file.expandableHoloMapRUIData[ closestAngleIdx ]
		}
		
		else if ( focusCandidate.topo != null )
		{
			focusCandidate = CryptoTT_CreateEmptyHoloMapRUIData()
		}

		
		if ( prevFocusCandidate.topo != focusCandidate.topo )
		{
			focusCandidateStartTime = Time()
			if ( IsValid( focusCandidate.topo ) && focusCandidate.canExpand)
			{
				RuiSetBool( focusCandidate.rui, "isFocused", true )
				RuiSetGameTime( focusCandidate.rui, "startFocusTime", Time() )
				if ( focusCandidate.topo != activatedPanel.topo )
					EmitSoundAtPosition( TEAM_UNASSIGNED, focusCandidate.origin, "Canyonlands_Cryto_TT_Holo_Icon_Focus" )
			}

			if ( IsValid( prevFocusCandidate.topo ) )
				RuiSetBool( prevFocusCandidate.rui, "isFocused", false )

		}
		else if ( ( focusCandidate.topo != null ) && ( Time() - focusCandidateStartTime > CRYPTO_MAP_FOCUS_TIME_TO_ACTIVATE ) )
		{
			
			if ( activatedPanel.topo != null && activatedPanel.topo != focusCandidate.topo )
			{
				CryptoTT_SetHoloMapRUIExpandedState( activatedPanel, false )
			}

			
			if ( activatedPanel.topo != focusCandidate.topo )
			{
				CryptoTT_SetHoloMapRUIExpandedState( focusCandidate, true )
				activatedPanel = focusCandidate
				activatedFocusCandidateStartTime = Time()
				focusCandidate = CryptoTT_CreateEmptyHoloMapRUIData()
			}
		}

		if ( focusCandidate.topo == activatedPanel.topo )
		{
			activatedFocusCandidateStartTime = Time()
		}

		
		if ( Time() - activatedFocusCandidateStartTime > CRYPTO_ACTIVATED_PANEL_INACTIVE_HIDE_TIME )
		{
			if ( activatedPanel.topo != null )
				CryptoTT_SetHoloMapRUIExpandedState( activatedPanel, false )

			activatedPanel = CryptoTT_CreateEmptyHoloMapRUIData()
			activatedFocusCandidateStartTime = Time()

			if ( IsValid( focusCandidate.topo ) )
				focusCandidateStartTime = Time()
		}

		WaitFrame()
	}
}

HoloMapRUIData function CryptoTT_CreateEmptyHoloMapRUIData()
{
	HoloMapRUIData newData
	return newData
}

void function CryptoTT_SetHoloMapRUIExpandedState( HoloMapRUIData ruiData, bool isExpanded )
{
	RuiSetBool( ruiData.rui, "isExpanded", isExpanded )
	ruiData.isExpanded = isExpanded

	if ( IsValid( ruiData.topo_FlyoutLine ) )
		RuiSetBool( ruiData.rui_FlyoutLine, "isVisible", isExpanded )

	if ( isExpanded )
	{
		RuiSetFloat( ruiData.rui, "fadeCloseDistance", CRYPTO_MAP_RUI_EXPANDED_LOOKING_AT_ORIENT_MIN_DIST)
		RuiSetFloat( ruiData.rui_FlyoutLine, "fadeCloseDistance", CRYPTO_MAP_RUI_EXPANDED_LOOKING_AT_ORIENT_MIN_DIST)

		vector expandedOrigin = CryptoTT_GenerateNewRuiExpandedOrigin( ruiData )
		ruiData.originExpanded = expandedOrigin

		RuiSetFloat3( ruiData.rui, "ruiOrigin", expandedOrigin )
		RuiSetFloat3( ruiData.rui_FlyoutLine, "ruiOrigin", expandedOrigin )

		EmitSoundAtPosition( TEAM_UNASSIGNED, ruiData.origin, "Canyonlands_Cryto_TT_Holo_Text_Expand" )
	}
	else
	{
		RuiSetFloat( ruiData.rui, "fadeCloseDistance", CRYPTO_MAP_RUI_LOOKING_AT_ORIENT_MIN_DIST )
		RuiSetFloat3( ruiData.rui, "ruiOrigin", ruiData.origin )
	}

	
	entity player = GetLocalViewPlayer()
	if ( IsValid( player ) )
		CryptoTT_HoloMap_OrientHoloRuis( player )
}

vector function CryptoTT_GenerateNewRuiExpandedOrigin( HoloMapRUIData ruiData )
{
	entity player = GetLocalViewPlayer()
	if ( !IsValid( player ) )
		return < 0, 0, 0 >

	vector forward     = player.CameraPosition() - ruiData.origin
	vector forwardNorm = Normalize( < forward.x, forward.y, 0 > )
	vector rightDir    = CrossProduct( forwardNorm, < 0, 0, 1 > )

	
	float rightOffset = CRYPTO_MAP_RUI_EXPANDED_OFFSET.x
	return ruiData.origin + ( rightDir * rightOffset )
}

HoloMapRUIData function CryptoTT_CreateAndRegisterHoloMapRUIData( vector origin, asset collapsedIcon, int colorIdx, bool canExpand )
{
	HoloMapRUIData newData

	newData.origin = origin
	vector topoRight =  < 0, 0, CRYPTO_MAP_TOPO_WIDTH >
	vector topoUp = < 0, 0, CRYPTO_MAP_TOPO_WIDTH >
	vector topoOrg = origin + ( topoRight * -0.5 ) + ( topoUp * 0.5 )
	newData.topo = RuiTopology_CreatePlane( topoOrg, topoRight, topoUp, true )

	newData.rui = RuiCreate( $"ui/crypto_tt_holo_map_icon_bunker.rpak", newData.topo, RUI_DRAW_WORLD, RuiCalculateDistanceSortKey( GetLocalViewPlayer().EyePosition(), origin ) )
	RuiSetImage( newData.rui, "collapsedIcon", collapsedIcon )
	RuiSetFloat3( newData.rui, "ruiOrigin", newData.origin )
	RuiSetInt( newData.rui, "widgetColorIndex", colorIdx )
	RuiSetBool( newData.rui, "isFocused", false )
	newData.canExpand = canExpand

	if ( canExpand )
	{
		
		topoRight =  < 0, 0, CRYPTO_MAP_TOPO_FLYOUT_WIDTH >
		topoUp = < 0, 0, CRYPTO_MAP_TOPO_HEIGHT >
		topoOrg = origin + ( topoRight * -0.5 ) + ( topoUp * 0.5 )
		newData.topo_FlyoutLine = RuiTopology_CreatePlane( topoOrg, topoRight, topoUp, true )

		newData.rui_FlyoutLine = RuiCreate( $"ui/crypto_tt_holo_map_flyout_line.rpak", newData.topo_FlyoutLine, RUI_DRAW_WORLD, RuiCalculateDistanceSortKey( GetLocalViewPlayer().EyePosition(), origin ) )
		RuiSetFloat3( newData.rui_FlyoutLine, "ruiOrigin", newData.origin )
		RuiSetInt( newData.rui_FlyoutLine, "widgetColorIndex", colorIdx )
		file.expandableHoloMapRUIData.append( newData )
	}

	file.allHoloMapRUIData.append( newData )

	return newData
}

void function CryptoTT_OnPlayerChangeLoadout( EHI playerEHI, ItemFlavor character )
{
	bool isCrypto = Localize( ItemFlavor_GetShortName( character ) ) == Localize( "#character_crypto_NAME_SHORT" )

	foreach( HoloMapRUIData ruiData in file.expandableHoloMapRUIData )
	{
		CryptoTT_UpdateHoloMapRUIText( ruiData, ruiData.hatchId, isCrypto )
	}
}

void function CryptoTT_UpdateHoloMapRUIText( HoloMapRUIData ruiData, string hatchId, bool isCrypto )
{
	string titleText
	string descText
	
	switch( hatchId )
	{
		case "16":
			titleText = "#CTT_BUNKER_NAME_Z16"
			descText = "#CTT_BUNKER_DESC_Z16"
			break
		case "6":
			titleText = "#CTT_BUNKER_NAME_Z6"
			descText = "#CTT_BUNKER_DESC_Z6"
			break
		case "5":
			titleText = "#CTT_BUNKER_NAME_Z5"
			descText = "#CTT_BUNKER_DESC_Z5"
			break
		case "12":
			titleText = "#CTT_BUNKER_NAME_Z12"
			descText = "#CTT_BUNKER_DESC_Z12"
			break
		case "12_treasure":
			titleText = "#CTT_BUNKER_NAME_Z12_MYSTERY"
			break
	}


	if ( !IsHatchBunkerUnlocked( hatchId ) )
	{
		descText = "#CTT_BUNKER_DESC_LOCKED"
	}
	else if ( !isCrypto )
		descText = "#CTT_BUNKER_DESC_NONCRYPTO"

	RuiSetString( ruiData.rui, "collapsedText", Localize( titleText ) )
	RuiSetString( ruiData.rui, "expandedTitleText", Localize( "#CTT_BUNKER_TITLE" ) )
	RuiSetString( ruiData.rui, "expandedDescTitleText", Localize( titleText ) )
	RuiSetString( ruiData.rui, "expandedDesc", Localize( descText ) )
}

asset function CryptoTT_GetIconAssetForBunker( string hatchId )
{
	if ( !IsHatchBunkerUnlocked( hatchId ) )
		return $"rui/hud/crypto_tt_holo_map/icon_crypto_tt_holomap_bunker_locked"

	switch( hatchId )
	{
		case "5":
			return $"rui/hud/crypto_tt_holo_map/icon_crypto_tt_holomap_bunker_z5"
		case "6":
			return $"rui/hud/crypto_tt_holo_map/icon_crypto_tt_holomap_bunker_z6"
		case "12":
			return $"rui/hud/crypto_tt_holo_map/icon_crypto_tt_holomap_bunker_z12"
		case "16":
			return $"rui/hud/crypto_tt_holo_map/icon_crypto_tt_holomap_bunker_z16"
	}

	unreachable
}

int function CryptoTT_GetColorIdxForBunker( string hatchId )
{
	if ( IsHatchBunkerUnlocked( hatchId ) )
		return eCryptoRUIColorIdxs.NEUTRAL
	else
		return eCryptoRUIColorIdxs.LOCKED

	unreachable
}

vector function CryptoTT_GetRuiOriginToUse( HoloMapRUIData ruiData )
{
	if ( ruiData.isExpanded )
		return ruiData.originExpanded
	else
		return ruiData.origin

	unreachable
}



bool function IsCryptoTTEnabled()
{
	if ( GetCurrentPlaylistVarBool( "crypto_map_scan_enabled", true ) )
	{
		return HasEntWithScriptName( "crypto_tt_holo_map_center" )
	}

	return false
}