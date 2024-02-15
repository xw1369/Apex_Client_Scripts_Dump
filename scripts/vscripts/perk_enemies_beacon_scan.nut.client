global function Perk_EnemyBeaconScan_Init













global function BeaconScanEnemy_ShowBeaconLocationOnMinimap
global function BeaconScanEnemy_ShowEnemiesOnMinimap
global function BeaconScanEnemy_ClearEnemiesOnMinimap
global function ServerToClient_BeaconScanEnemy_Notifications
global function PlayEffects_SurveyBeacon_Laser
global function StopEffects_SurveyBeacon_Laser


struct
{







		array< var > fullMapRuis
		array< var > minimapRuis

} file

void function Perk_EnemyBeaconScan_Init()
{

		Remote_RegisterClientFunction( "BeaconScanEnemy_ShowEnemiesOnMinimap", "entity", "entity", "entity", "float", -1.0, 16000.0, 15, "float", -1.0, 60.0, 6  )
		Remote_RegisterClientFunction( "BeaconScanEnemy_ShowBeaconLocationOnMinimap", "entity", "entity" )
		Remote_RegisterClientFunction( "ServerToClient_BeaconScanEnemy_Notifications", "entity", "entity" )
		Remote_RegisterClientFunction( "StopEffects_SurveyBeacon_Laser", "entity", "entity" )


	if ( !(GetCurrentPlaylistVarBool( "disable_perk_beacon_scan_enemy", false ) ) )
	{
		PerkInfo beaconScanEnemy
		beaconScanEnemy.perkId          = ePerkIndex.BEACON_ENEMY_SCAN

			beaconScanEnemy.activateCallback = OnActivate_BeaconScan_Enemy
			beaconScanEnemy.deactivateCallback = OnDeactivate_BeaconScan
			beaconScanEnemy.minimapStateIndex = eMinimapObject_prop_script.SURVEY_BEACON
			beaconScanEnemy.minimapPingType = ePingType.SURVEYBEACON
			beaconScanEnemy.mapFeatureTitle = "#PERK_FEATURE_SURVEY_BEACON"
			beaconScanEnemy.mapFeatureDescription = "#PERK_FEATURE_SURVEY_BEACON_DESC"


			beaconScanEnemy.worldspaceIconUpOffset = 96

		Perks_RegisterClassPerk( beaconScanEnemy )


		if( EnemyBeaconScan_UseNewBeaconModel() )
		{
			PrecacheModel( $"mdl/props/recon_beacon_dish/recon_beacon_dish.rmdl" )
		}


			AddCallback_OnPassiveChanged( ePassives.PAS_UPGRADE_BEACON_SCAN, OnPassiveChangedBeaconScanUpgrade )






	}

	bool shouldUseSeperateNextZoneScanProp = SurveyBeacons_ShouldUseNextZoneSurveyBeaconProp()
	if ( !(GetCurrentPlaylistVarBool( "disable_perk_beacon_scan", false ) ) && !shouldUseSeperateNextZoneScanProp )
	{
		PerkInfo beaconScan
		beaconScan.perkId          = ePerkIndex.BEACON_SCAN

			beaconScan.activateCallback = OnActivate_BeaconScan_Circle
			beaconScan.deactivateCallback = OnDeactivate_BeaconScan


			beaconScan.worldspaceIconUpOffset = 96

		Perks_RegisterClassPerk( beaconScan )
	}
}



void function OnPassiveChangedBeaconScanUpgrade( entity player, int passive, bool didHave, bool nowHas )
{
	if( nowHas )
	{
		Perks_AddPerk( player, ePerkIndex.BEACON_ENEMY_SCAN )
	}
}



bool function EnemyBeaconScan_UseNewBeaconModel()
{
	return GetCurrentPlaylistVarBool( "perk_enemy_beacon_use_new_model", true )
}

bool function EnemyBeaconScan_RevealScannerLocation()
{
	return GetCurrentPlaylistVarBool( "perk_enemy_beacon_use_scanner_location", true )
}


float function EnemyBeaconScan_WarningRange()
{
	return GetCurrentPlaylistVarFloat( "perk_enemy_beacon_warning_range", 15000 )
}

float function EnemyBeaconScan_GetBeaconScanDuration()
{
	return GetCurrentPlaylistVarFloat( "perk_enemy_beacon_scan_duration", 30.0 )
}










void function OnActivate_BeaconScan_Enemy( entity player, string characterName )
{
	string calloutLine, player1pAnim, player3pAnim, panelAnim

	calloutLine  = "bc_revealEnemyPosition"

	switch ( characterName )
	{
		case "pathfinder":
		{
			player1pAnim = "pathfinder"
			player3pAnim = "pathfinder"
			panelAnim    = "pathfinder"
			break
		}
		case "crypto":
		{
			if ( HasCryptoSword ( player ) )
			{
				player1pAnim = "crypto_heirloom"
				panelAnim    = "crypto_heirloom"				
				player3pAnim = "crypto"
			}
			else
			{
				player3pAnim = "crypto"
			}
			break
		}
		case "seer":
		{
				if ( HasSeerBlades ( player ) )
				{
					player1pAnim = "seer_heirloom"
					panelAnim    = "seer_heirloom"
					
					
				}
				else
				{
					player1pAnim = "seer"
				}
			break
		}
		case "vantage":
		{
			player1pAnim = "vantage"
			panelAnim    = "vantage"
			break
		}

		default:
			calloutLine  = "bc_revealEnemyPosition"
			break
	}

	RegisterEnemySurveyBeaconData( player, calloutLine, player1pAnim, player3pAnim, panelAnim )
}

void function RegisterEnemySurveyBeaconData(  entity player, string calloutLine, string player1pAnim, string player3pAnim, string panelAnim )
{
	SurveyBeaconData data
	data.canUseFunc = BeaconScanEnemy_CanUseBeacon



	data.calloutLine = calloutLine
	data.scanType = eBeaconScanType.BEACON_SCAN_ENEMY


























	RegisterSurveyBeaconData( player, data )
}

bool function BeaconScanEnemy_CanUseBeacon( entity player, entity beacon )
{
	
	bool isUsable

	int minimapFlags = beacon.Minimap_GetRuiMinimapFlags()
	isUsable = ( minimapFlags & MINIMAP_FLAG_VISIBILITY_SHOW ) > 0





	if ( !isUsable )
	{
		return false
	}

	if( beacon.GetScriptName() != ENEMY_SURVEY_BEACON_SCRIPTNAME )
		return false

	return true
}




















































































































































































































void function BeaconScanEnemy_ShowBeaconLocationOnMinimap( entity enemy, entity beacon )
{
	if ( enemy != GetLocalViewPlayer() )
		return

	vector pulseOrigin = beacon.GetOrigin()
	FullMap_PlayCryptoPulseSequence( pulseOrigin, false, EnemyBeaconScan_GetBeaconScanDuration() ) 
}

void function BeaconScanEnemy_ShowEnemiesOnMinimap( entity playerWhoScanned, entity player, entity beacon, float scanRangeParm, float scanDurationParm )
{
	if ( player != GetLocalViewPlayer() )
		return

	vector playerNameColor = <255, 255, 255>

	int teamMemberIndex = playerWhoScanned.GetTeamMemberIndex()
	if ( teamMemberIndex < 0 )
		Warning( "%s() - Invalid team member index (%d) for player: %s", FUNC_NAME(), teamMemberIndex, string( player ) )
	else
		playerNameColor = GetPlayerInfoColor( playerWhoScanned )

	string playerName = playerWhoScanned == player ? Localize( "#OBITUARY_YOU" ) : playerWhoScanned.GetPlayerName()

	Obituary_Print_Localized( Localize( "#SURVIVAL_HUD_REVEALED_ENEMIES", playerName ), playerNameColor )
	thread BeaconScanEnemy_DisplayEnemiesOnMinimap_Thread( player, beacon, scanRangeParm, scanDurationParm )
}

void function BeaconScanEnemy_DisplayEnemiesOnMinimap_Thread( entity player, entity beacon, float scanRangeParm, float scanDurationParm )
{
	if ( player != GetLocalViewPlayer() )
		return

	int team = player.GetTeam()
	array<entity> aliveEnemies = GetPlayerArrayOfEnemies_Alive( team )

	
	if( scanRangeParm > 0 )
	{
		float scanRange = scanRangeParm > 0 ? scanRangeParm : EnemyBeaconScan_WarningRange()
		float scanRangeSqr = scanRange * scanRange
		foreach( enemy in aliveEnemies )
		{
			float distToBeaconSqr = Distance2DSqr( enemy.GetOrigin(), beacon.GetOrigin() )
			if( distToBeaconSqr > scanRangeSqr )
			{
				aliveEnemies.removebyvalue( enemy )
			}
		}
	}

	float scanDuration = scanDurationParm > 0 ? scanDurationParm : EnemyBeaconScan_GetBeaconScanDuration()
	float endTime = Time() + scanDuration
	float timeToStartFade = Time() + ( scanDuration/2 ) 
	float timeToEndFade = endTime

	array<entity> entsForTracking

	OnThreadEnd(
		function() : ( player )
		{
			BeaconScanEnemy_ClearEnemiesOnMinimap( player )
		}
	)

	foreach( entity enemy in aliveEnemies )
	{





		
		var fRui = FullMap_AddEnemyLocation( enemy )
		file.fullMapRuis.append( fRui )

		
		var mRui = Minimap_AddEnemyToMinimap( enemy )
		file.minimapRuis.append( mRui )
		RuiSetGameTime( mRui, "fadeStartTime", timeToStartFade )
		RuiSetGameTime( mRui, "fadeEndTime", timeToEndFade )
	}

	vector pulseOrigin = beacon.GetOrigin()

	FullMap_PlayCryptoPulseSequence( pulseOrigin, true, EnemyBeaconScan_GetBeaconScanDuration() )

	while ( Time() < endTime ) 
	{
		if( !IsValid(player) )
			break
		WaitFrame()
	}
}

void function BeaconScanEnemy_ClearEnemiesOnMinimap( entity player )
{
	if ( player != GetLocalViewPlayer() )
		return

	foreach( var ruiToDestroy in file.fullMapRuis )
	{
		Fullmap_RemoveRui( ruiToDestroy )
		RuiDestroyIfAlive( ruiToDestroy )
	}
	file.fullMapRuis = []

	foreach( var ruiToDestroy in file.minimapRuis)
	{
		Minimap_CommonCleanup( ruiToDestroy )
	}
	file.minimapRuis = []
}

void function ServerToClient_BeaconScanEnemy_Notifications( entity player, entity beacon )
{
	int team = player.GetTeam()
	
	bool showSquadInfo =  GetCurrentPlaylistVarBool( "beacon_show_squad_info", false )
	if (showSquadInfo)
		SurveyBeacon_ShowSquadInfo()

	EmitSoundOnEntity( beacon, "Recon_SurveyBeacon_EnemiesScanned_UI_1P" )
}




void function PlayEffects_SurveyBeacon_Laser( entity beacon )
{
	EmitSoundOnEntity( beacon, "Recon_Hack_SurveyBeacon_LaserBeam_3P")
}


void function StopEffects_SurveyBeacon_Laser( entity player, entity soundEnt )
{
	StopSoundOnEntity( soundEnt, "Recon_Hack_SurveyBeacon_LaserBeam_3P" )
	StopSoundOnEntity( soundEnt, "Recon_Hack_SurveyBeacon_LaserBeam_Stereo_3P" )
}

