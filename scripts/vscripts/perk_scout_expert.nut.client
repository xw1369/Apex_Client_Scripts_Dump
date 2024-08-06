global function Perk_ScoutExpert_Init
global function Perk_ScoutExpert_FastBeaconScan_Enabled
global function Perk_ScoutExpert_FastBeaconScan_ScanRange
global function Perk_ScoutExpert_FastBeaconScan_ScanDuration
global function Perk_ScoutExpert_RescanWait









global function Perk_ScoutExpert_FastBeaconScan_RuiThread


const string INNATE_THREAT_VISION_MOD = "innate_threat_vision"
const asset FAST_BEACON_SCAN_START_FX = $"P_beacon_Survey_Pulse"

struct
{

	array<entity> surveyBeacons

}file

struct
{
	float beaconScanRange = 500
	float surveyBeaconScanRange = 20000
	bool refundTacOnBeaconScan = false
	float rescanWait = 5
	float satelliteScanDuration = 15
	float companionScanTime = 10
}tunings

void function Perk_ScoutExpert_Init()
{
	PerkInfo scoutExpert
	scoutExpert.perkId          = ePerkIndex.SCOUT_EXPERT

	if( Perk_ScoutExpert_IsEnabled() )
	{
		scoutExpert.activateCallback = Perk_ScoutExpert_Activate
		scoutExpert.deactivateCallback = Perk_ScoutExpert_Deactivate
	}

	PrecacheParticleSystem( FAST_BEACON_SCAN_START_FX )
	Remote_RegisterServerFunction( "Perk_ScoutExpert_TriggerBeaconScan", "typed_entity", "prop_script" )



		AddCreateCallback( "prop_script", SurveyBeaconCreated )
		AddDestroyCallback( "prop_script", SurveyBeaconDestroyed )


	Perks_RegisterClassPerk( scoutExpert )

	RegisterSignal( "ScoutExpertEnd" )

	tunings.beaconScanRange = GetCurrentPlaylistVarFloat( "perk_scout_expert_fast_beacon_scan_range", tunings.beaconScanRange )
	tunings.refundTacOnBeaconScan = GetCurrentPlaylistVarBool( "perk_scout_expert_fast_beacon_scan_refund_tac", tunings.refundTacOnBeaconScan )
	tunings.surveyBeaconScanRange = GetCurrentPlaylistVarFloat( "perk_scout_expert_fast_beacon_survey_beacon_scan_range", tunings.surveyBeaconScanRange )
	tunings.rescanWait = GetCurrentPlaylistVarFloat( "perk_scout_expert_fast_beacon_rescan_wait", tunings.rescanWait )
	tunings.satelliteScanDuration = GetCurrentPlaylistVarFloat( "perk_scout_expert_fast_beacon_scan_duration", tunings.satelliteScanDuration )
	tunings.companionScanTime = GetCurrentPlaylistVarFloat( "perk_scout_expert_fast_beacon_companion_scan_time", tunings.companionScanTime )
}

float function Perk_ScoutExpert_RescanWait()
{
	return tunings.rescanWait
}

bool function Perk_ScoutExpert_FastBeaconScan_Enabled()
{
	return Perks_S22UpdateEnabled() && GetCurrentPlaylistVarBool( "perk_scout_expert_fast_beacon_scan_enabled", true )
}

bool function Perk_ScoutExpert_IsEnabled()
{
	return Perks_S22UpdateEnabled() && GetCurrentPlaylistVarBool( "perk_scout_expert_is_enabled", true )
}

float function Perk_ScoutExpert_FastBeaconScan_ScanRange()
{
	return tunings.surveyBeaconScanRange
}

float function Perk_ScoutExpert_FastBeaconScan_ScanDuration()
{
	return tunings.satelliteScanDuration
}


void function Perk_ScoutExpert_Activate( entity player, string characterName )
{




	

}

void function Perk_ScoutExpert_Deactivate( entity player )
{




	player.Signal( "ScoutExpertEnd" )

}






















































































void function SurveyBeaconCreated( entity ent )
{
	if( ent.GetScriptName() != ENEMY_SURVEY_BEACON_SCRIPTNAME )
		return

	file.surveyBeacons.append( ent )
}

void function SurveyBeaconDestroyed( entity ent )
{
	if( ent.GetScriptName() != ENEMY_SURVEY_BEACON_SCRIPTNAME )
		return

	if( file.surveyBeacons.contains( ent ) )
		file.surveyBeacons.fastremovebyvalue( ent )
}

void function Perk_ScoutExpert_TrackCompanionScan( entity player )
{
	if( GetLocalViewPlayer() != player )
		return

	if( !Perk_ScoutExpert_FastBeaconScan_Enabled() || tunings.beaconScanRange <= 0.0 )
		return

	if( tunings.companionScanTime <= 0 )
		return

	player.EndSignal( "OnDestroy", "OnDeath", "ScoutExpertEnd" )

	entity scanningBeacon = null
	while( true )
	{
		entity companion = player.p.cryptoActiveCamera
		if( companion == null )
		{
			companion =	VantageCompanion_GetEnt( player )
			if( companion != null && player.GetPlayerNetInt( VANTAGE_COMPANION_STATE_NETINT ) == eCompanionState.PERCHED )
			{
				companion = null
			}
		}

		if( companion != null )
		{
			array<entity> beacons = file.surveyBeacons
			array<entity> candidateBeacons = GetEntitiesFromArrayNearPos( beacons, companion.GetOrigin(), tunings.beaconScanRange )
			bool foundNearbyBeacon = false
			foreach( entity beacon in candidateBeacons )
			{
				if( !SurveyBeacon_CanActivate( player, beacon ) )
					continue

				
				if( beacon != scanningBeacon )
				{
					if( scanningBeacon != null )
					{
						BeaconScanCompanion_SetScanStartTime( scanningBeacon, -1 )
					}
					scanningBeacon = beacon
					BeaconScanCompanion_SetScanStartTime( scanningBeacon, Time() )
				}

				float scanStartTime = BeaconScanCompanion_GetScanStartTime( beacon )
				if( scanStartTime > 0 && Time() > scanStartTime + tunings.companionScanTime )
				{
					BeaconScanCompanion_SetScanStartTime( scanningBeacon, -1 )
					Remote_ServerCallFunction( "Perk_ScoutExpert_TriggerBeaconScan", scanningBeacon )
					scanningBeacon = null
				}
				foundNearbyBeacon = true
				break
			}

			if( !foundNearbyBeacon && scanningBeacon != null )
			{
				BeaconScanCompanion_SetScanStartTime( scanningBeacon, -1 )
				scanningBeacon = null
			}
		}
		else if( scanningBeacon != null )
		{
			BeaconScanCompanion_SetScanStartTime( scanningBeacon, -1 )
			scanningBeacon = null
		}
		WaitFrame()
	}
}

void function Perk_ScoutExpert_FastBeaconScan_RuiThread( var rui, entity ent )
{
	ent.EndSignal( "OnDestroy", "HidePerkMinimapVisibility" )
	clGlobal.levelEnt.EndSignal( "UpdatePerkMinimapVisibility" )
	entity localPlayer = GetLocalViewPlayer()
	if( !Perks_DoesPlayerHavePerk( localPlayer, ePerkIndex.BEACON_ENEMY_SCAN ) )
	{
		return
	}

	float closeThreshold = METERS_TO_INCHES * 50

	while( true )
	{
		if( DistanceSqr( localPlayer.GetOrigin(), ent.GetOrigin() ) < closeThreshold * closeThreshold )
		{
			RuiSetFloat3( rui, "iconColor", <1, 1, 0> )
		}
		else
		{
			RuiSetFloat3( rui, "iconColor", <1, 1, 1> )
		}

		float scanStartTime = BeaconScanCompanion_GetScanStartTime( ent )
		if( scanStartTime > 0 )
		{
			float scanFrac = ( Time() - scanStartTime ) / tunings.companionScanTime
			RuiSetFloat( rui, "arcFill", scanFrac )
			RuiSetBool( rui, "isUnrevealedCarePackageInsight", true )
		}
		else
		{
			RuiSetBool( rui, "isUnrevealedCarePackageInsight", false )
		}

		WaitFrame()
	}
}

