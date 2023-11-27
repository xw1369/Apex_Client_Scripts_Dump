global function Sh_RankedTrials_Init
global function RankedTrials_GetTrialToEnterTier
global function RankedTrials_RegisterAllRankedTrials


global function RankedTrials_GetTrialMatchCount
global function RankedTrials_GetTrialMaxMatchCount
global function RankedTrials_GetTrialBonusLPGain
global function RankedTrials_GetTrialMaxLPLoss
global function RankedTrials_SecondaryTrialRequiresSingleMatchPerformance
global function RankedTrials_GetSecondaryTrialSingleMatchComboCount
global function RankedTrials_GetTrialStatGoalByIndex
global function RankedTrials_GetTrialStatEntryByIndex
global function RankedTrials_HasSecondaryTrial
global function RankedTrials_HasDualStatSecondaryTrial


global function RankedTrials_PlayerHasAssignedTrial
global function RankedTrials_GetAssignedTrial
global function RankedTrials_GetNetLP
global function RankedTrials_GetSecondaryStatMatchComboStatProgress
global function RankedTrials_GetGamesPlayedInTrialsState
global function RankedTrials_GetGamesAllowedInTrialsState
global function RankedTrials_GetTimesFailedTrial
global function RankedTrials_GetProgressValueForStatByIndex
global function RankedTrials_GetTrialState


global function RankedTrials_GetDescription
global function RankedTrials_GetTrialsCountForTrial
global function RankedTrials_PlayerHasIncompleteTrial
global function RankedTrials_NextRankHasTrial











#if DEV



#endif

global const string RANKED_TRIALS_PERSISTENCE_STATE_KEY = "trialState"

global enum eRankedTrialState
{
	NOT_IN_TRIAL,
	INCOMPLETE,
	SUCCESS,
	FAILURE,
	COUNT,
}

global enum eRankedTrialGoalIdx
{
	PRIMARY,
	SECONDARY_ONE,
	SECONDARY_TWO,
}

struct
{
	table< string, int > rankedTierToTrialMap 
	table< int, array< string > > trialStatRefCache 
} file

const string CONVAR_KILL_SWITCH = "ranked_disable_promo_trials"


const string R2P0_SETTINGS_KEY_RANKED_TRIAL = "rankedTrialToEnterTier" 
const string SETTINGS_KEY_ENTERS_TIER = "entersTier" 
const string SETTINGS_KEY_MATCH_COUNT = "matchCount"
const string SETTINGS_KEY_MATCH_COUNT_MAX = "matchCountMax"
const string SETTINGS_KEY_BONUS_LP_GAIN = "bonusLPGain"
const string SETTINGS_KEY_MAX_LP_LOSS = "maxLPLoss"
const string SETTINGS_KEY_PRIMARY_GOAL_VAL = "goalVal" 
const string SETTINGS_KEY_PRIMARY_STAT_REF = "statRef" 
const string SETTINGS_KEY_SECONDARY_REQUIRES_SINGLE_MATCH = "singleMatch" 
const string SETTINGS_KEY_SECONDARY_SINGLE_MATCH_COMBO_COUNT = "singleMatchComboCount" 
const string SETTINGS_KEY_SECONDARY_GOAL_VAL_ONE = "goalValAltOne" 
const string SETTINGS_KEY_SECONDARY_STAT_REF_ONE = "statRefAltOne" 
const string SETTINGS_KEY_SECONDARY_GOAL_VAL_TWO = "goalValAltTwo" 
const string SETTINGS_KEY_SECONDARY_STAT_REF_TWO = "statRefAltTwo" 
const string SETTINGS_KEY_DESCRIPTION_PRIMARY = "description" 
const string SETTINGS_KEY_DESCRIPTION_SECONDARY_ONE = "descriptionAltOne" 
const string SETTINGS_KEY_DESCRIPTION_SECONDARY_TWO = "descriptionAltTwo" 

const string PLAYLIST_OVERRIDE_FORMAT_STRING = "ranked_trials_%s_%s" 


const string RANKED_TRIALS_PERSISTENCE_STRUCT = "rankedTrials"
const string PERSISTENCE_KEY_FORMAT_STRING = XPROG_PERSISTENCE_PREFIX_FORMAT_STRING + RANKED_TRIALS_PERSISTENCE_STRUCT + ".%s"
const string PERSISTENCE_KEY_GUID = "guid" 
const string PERSISTENCE_KEY_PRIMARY_STAT_PROGRESS = "primaryStatProgress" 
const string PERSISTENCE_KEY_SECONDARY_STAT_PROGRESS_ONE = "secondaryStatProgressOne" 
const string PERSISTENCE_KEY_SECONDARY_STAT_PROGRESS_TWO = "secondaryStatProgressTwo" 
const string PERSISTENCE_KEY_SECONDARY_STAT_PROGRESS_COMBO = "secondaryTrialCombinedProgress" 
const string PERSISTENCE_KEY_GAMES_IN_TRIAL_STATE = "gamesPlayedInTrialState"
const string PERSISTENCE_KEY_NET_LP_DURING_TRIAL = "netLPDuringTrial" 
const string PERSISTENCE_KEY_TRIAL_STATE = "state" 
const string PERSISTENCE_KEY_TIMES_FAILED_TRIAL = "timesFailedTrial" 

const array< string > PERSISTENCE_KEYS =
[
	PERSISTENCE_KEY_GUID,
	PERSISTENCE_KEY_PRIMARY_STAT_PROGRESS,
	PERSISTENCE_KEY_SECONDARY_STAT_PROGRESS_ONE,
	PERSISTENCE_KEY_SECONDARY_STAT_PROGRESS_TWO,
	PERSISTENCE_KEY_SECONDARY_STAT_PROGRESS_COMBO,
	PERSISTENCE_KEY_GAMES_IN_TRIAL_STATE,
	PERSISTENCE_KEY_NET_LP_DURING_TRIAL,
	PERSISTENCE_KEY_TRIAL_STATE,
	PERSISTENCE_KEY_TIMES_FAILED_TRIAL, 
]

const table< string, string > BAKERY_LABEL_TO_STAT_REF =
{
	["PLACEMENTS_WIN"] = "stats.rankedperiods[%s].placements_win",
	["PLACEMENTS_TOP_5"] = "stats.rankedperiods[%s].placements_top_5",
	["PLACEMENTS_TOP_10"] = "stats.rankedperiods[%s].placements_top_10",
	["KILLS_OR_ASSISTS"] = "stats.kills_or_assists", 
	["KILLS"] = "stats.rankedperiods[%s].kills",
	["DAMAGE_DONE"] = "stats.rankedperiods[%s].damage_done",
	["NONE"] = "",
}

#if DEV
const string DEV_RUN_PLAYLIST_OVERRIDE_CHECK = "ranked_trials_dev_playlist_check"
const int DEV_FORCE_COMPLETION_STATUS = -1 
#endif

void function Sh_RankedTrials_Init()
{
	_CacheStatEntries() 
#if DEV
		Assert( PERSISTENCE_KEYS.find( PERSISTENCE_KEY_TIMES_FAILED_TRIAL ) == PERSISTENCE_KEYS.len() - 1 )
		Assert( DEV_CheckPlaylistOverrides() )
#endif
}

void function RankedTrials_RegisterAllRankedTrials()
{
	ItemFlavor ornull currentRankedPeriod = Ranked_GetCurrentActiveRankedPeriod()
	if ( currentRankedPeriod == null )
		return

	expect ItemFlavor( currentRankedPeriod )
	if ( ItemFlavor_GetType( currentRankedPeriod ) != eItemType.ranked_2pt0_period )
		return

	foreach ( var tierBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( currentRankedPeriod ), "tiers" ) )
	{
		asset trialAsset = GetSettingsBlockAsset( tierBlock, R2P0_SETTINGS_KEY_RANKED_TRIAL )
		if ( trialAsset != $"" )
		{
			ItemFlavor ornull rankedTrial = RegisterItemFlavorFromSettingsAsset( trialAsset )
			if ( rankedTrial == null )
			{
				printt( "Failed to register ItemFlavor from Settings Asset ", trialAsset )
			}
			else
			{
				expect ItemFlavor( rankedTrial )
				var settingsBlock = ItemFlavor_GetSettingsBlock( rankedTrial )
				string entersTier = GetSettingsBlockString( settingsBlock, SETTINGS_KEY_ENTERS_TIER )
				Assert( TIER_ORDERING_BY_LOC_KEY.contains( entersTier ) )
				file.rankedTierToTrialMap[ entersTier ] <- ItemFlavor_GetGUID( rankedTrial )
			}
		}
	}
}

ItemFlavor function RankedTrials_GetTrialToEnterTier( string tierName )
{
	Assert( tierName in file.rankedTierToTrialMap )
	Assert( IsValidItemFlavorGUID( file.rankedTierToTrialMap[ tierName ] ) )
	return GetItemFlavorByGUID( file.rankedTierToTrialMap[ tierName ] )
}


int function RankedTrials_GetTrialMatchCount( ItemFlavor rankedTrial )
{
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )

	var settingsBlock = ItemFlavor_GetSettingsBlock( rankedTrial )
	int matchCount    = GetSettingsBlockInt( settingsBlock, SETTINGS_KEY_MATCH_COUNT )

	return GetCurrentPlaylistVarInt( format( PLAYLIST_OVERRIDE_FORMAT_STRING, ItemFlavor_GetGUIDString( rankedTrial ), SETTINGS_KEY_MATCH_COUNT ), matchCount )
}

int function RankedTrials_GetTrialMaxMatchCount( ItemFlavor rankedTrial )
{
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )

	var settingsBlock = ItemFlavor_GetSettingsBlock( rankedTrial )
	int matchCountMax = GetSettingsBlockInt( settingsBlock, SETTINGS_KEY_MATCH_COUNT_MAX )

	return GetCurrentPlaylistVarInt( format( PLAYLIST_OVERRIDE_FORMAT_STRING, ItemFlavor_GetGUIDString( rankedTrial ), SETTINGS_KEY_MATCH_COUNT_MAX ), matchCountMax )
}

int function RankedTrials_GetTrialBonusLPGain( ItemFlavor rankedTrial )
{
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )

	var settingsBlock = ItemFlavor_GetSettingsBlock( rankedTrial )
	int maxLpGain     = GetSettingsBlockInt( settingsBlock, SETTINGS_KEY_BONUS_LP_GAIN )

	return GetCurrentPlaylistVarInt( format( PLAYLIST_OVERRIDE_FORMAT_STRING, ItemFlavor_GetGUIDString( rankedTrial ), SETTINGS_KEY_BONUS_LP_GAIN ), maxLpGain )
}

int function RankedTrials_GetTrialMaxLPLoss( ItemFlavor rankedTrial )
{
	
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )

	var settingsBlock = ItemFlavor_GetSettingsBlock( rankedTrial )
	int maxLpLoss     = GetSettingsBlockInt( settingsBlock, SETTINGS_KEY_MAX_LP_LOSS )
	Assert( maxLpLoss > 0 )
	int maxLpLossPl   = GetCurrentPlaylistVarInt( format( PLAYLIST_OVERRIDE_FORMAT_STRING, ItemFlavor_GetGUIDString( rankedTrial ), SETTINGS_KEY_MAX_LP_LOSS ), maxLpLoss )
	Assert( maxLpLossPl > 0 )
	return ( -1 * abs( maxLpLossPl ) )
}

bool function RankedTrials_SecondaryTrialRequiresSingleMatchPerformance( ItemFlavor rankedTrial )
{
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )

	var settingsBlock        = ItemFlavor_GetSettingsBlock( rankedTrial )
	bool requiresSingleMatch = GetSettingsBlockBool( settingsBlock, SETTINGS_KEY_SECONDARY_REQUIRES_SINGLE_MATCH )

	return GetCurrentPlaylistVarBool( format( PLAYLIST_OVERRIDE_FORMAT_STRING, ItemFlavor_GetGUIDString( rankedTrial ), SETTINGS_KEY_SECONDARY_REQUIRES_SINGLE_MATCH ), requiresSingleMatch )
}

int function RankedTrials_GetSecondaryTrialSingleMatchComboCount( ItemFlavor rankedTrial )
{
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )

	var settingsBlock   = ItemFlavor_GetSettingsBlock( rankedTrial )
	int matchComboCount = GetSettingsBlockInt( settingsBlock, SETTINGS_KEY_SECONDARY_SINGLE_MATCH_COMBO_COUNT )

	return GetCurrentPlaylistVarInt( format( PLAYLIST_OVERRIDE_FORMAT_STRING, ItemFlavor_GetGUIDString( rankedTrial ), SETTINGS_KEY_SECONDARY_SINGLE_MATCH_COMBO_COUNT ), matchComboCount )
}

int function RankedTrials_GetTrialStatGoalByIndex( ItemFlavor rankedTrial, int statIdx )
{
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )

	string keyStr = ""
	switch ( statIdx )
	{
		case eRankedTrialGoalIdx.PRIMARY:
			keyStr = SETTINGS_KEY_PRIMARY_GOAL_VAL
			break

		case eRankedTrialGoalIdx.SECONDARY_ONE:
			keyStr = SETTINGS_KEY_SECONDARY_GOAL_VAL_ONE
			break

		case eRankedTrialGoalIdx.SECONDARY_TWO:
			keyStr = SETTINGS_KEY_SECONDARY_GOAL_VAL_TWO
			break
	}
	Assert( keyStr != "" )

	var settingsBlock = ItemFlavor_GetSettingsBlock( rankedTrial )
	int statGoal      = GetSettingsBlockInt( settingsBlock, keyStr )

	return GetCurrentPlaylistVarInt( format( PLAYLIST_OVERRIDE_FORMAT_STRING, ItemFlavor_GetGUIDString( rankedTrial ), keyStr ), statGoal )
}

bool function RankedTrials_HasSecondaryTrial( ItemFlavor rankedTrial )
{
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )
	return file.trialStatRefCache[ ItemFlavor_GetGUID( rankedTrial ) ].isvalidindex( eRankedTrialGoalIdx.SECONDARY_ONE )
}

bool function RankedTrials_HasDualStatSecondaryTrial( ItemFlavor rankedTrial )
{
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )
	return file.trialStatRefCache[ ItemFlavor_GetGUID( rankedTrial ) ].isvalidindex( eRankedTrialGoalIdx.SECONDARY_TWO )
}


int function RankedTrials_GetTrialsCountForTrial( ItemFlavor rankedTrial )
{
	int trialsCount = 1
	bool hasSecondaryTrial = RankedTrials_HasSecondaryTrial( rankedTrial )
	bool dualStatSecondary = hasSecondaryTrial && RankedTrials_HasDualStatSecondaryTrial( rankedTrial )  && !RankedTrials_SecondaryTrialRequiresSingleMatchPerformance( rankedTrial )
	if ( hasSecondaryTrial )
	{
		trialsCount += dualStatSecondary ? 2: 1
	}
	return trialsCount
}


void function _CacheStatEntries()
{
	foreach ( string _, int rankedTrialGUID in file.rankedTierToTrialMap )
	{
		Assert( IsValidItemFlavorGUID( rankedTrialGUID ) )
		ItemFlavor rankedTrial = GetItemFlavorByGUID( rankedTrialGUID )
		array< string > statArray = []
		foreach ( int statIdx in eRankedTrialGoalIdx )
		{
			string keyStr = ""
			switch ( statIdx )
			{
				case eRankedTrialGoalIdx.PRIMARY:
					keyStr = SETTINGS_KEY_PRIMARY_STAT_REF
					break

				case eRankedTrialGoalIdx.SECONDARY_ONE:
					keyStr = SETTINGS_KEY_SECONDARY_STAT_REF_ONE
					break

				case eRankedTrialGoalIdx.SECONDARY_TWO:
					keyStr = SETTINGS_KEY_SECONDARY_STAT_REF_TWO
					break
			}
			Assert( keyStr != "" )

			var settingsBlock = ItemFlavor_GetSettingsBlock( rankedTrial )
			string statRefKey = GetSettingsBlockString( settingsBlock, keyStr )
			string statRef    = format( BAKERY_LABEL_TO_STAT_REF[ statRefKey ], Ranked_GetCurrentPeriodGUIDString() )
			Assert( ( statRef == "" && statIdx >= eRankedTrialGoalIdx.SECONDARY_ONE ) || IsValidStatEntryRef( statRef ) )

			string playlistRef = GetCurrentPlaylistVarString( format( PLAYLIST_OVERRIDE_FORMAT_STRING, ItemFlavor_GetGUIDString( rankedTrial ), keyStr ), statRef )
			if ( IsValidStatEntryRef( playlistRef ) )
			{
				statArray.append( playlistRef )
			}
			else if ( statRef != "" )
			{
				Assert( false, format( "Ranked Trials: Playlist override is broken for Trial %d - invalid statRef %s", ItemFlavor_GetGUID( rankedTrial ), playlistRef ) )
				statArray.append( statRef )
			}
			else
			{
				Assert( statIdx >= eRankedTrialGoalIdx.SECONDARY_ONE )
				break
			}
		}
		file.trialStatRefCache[ ItemFlavor_GetGUID( rankedTrial ) ] <- statArray
	}
	Assert( file.trialStatRefCache.len() == file.rankedTierToTrialMap.len() )
}

StatEntry function RankedTrials_GetTrialStatEntryByIndex( ItemFlavor rankedTrial, int statIdx )
{
	int guid = ItemFlavor_GetGUID( rankedTrial )
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )
	Assert( statIdx == eRankedTrialGoalIdx.PRIMARY || RankedTrials_HasSecondaryTrial( rankedTrial ) )
	Assert( statIdx != eRankedTrialGoalIdx.SECONDARY_TWO || RankedTrials_HasDualStatSecondaryTrial( rankedTrial ) )
	Assert( _IsValidTrialStatEntryIndex( rankedTrial, statIdx ) )
	Assert( IsValidStatEntryRef( file.trialStatRefCache[ guid ][ statIdx ] ) )

	return GetStatEntryByRef( file.trialStatRefCache[ guid ][ statIdx ] )
}

bool function _IsValidTrialStatEntryIndex( ItemFlavor rankedTrial, int statIdx )
{
	int guid = ItemFlavor_GetGUID( rankedTrial )
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )
	Assert( guid in file.trialStatRefCache )

	return file.trialStatRefCache[ guid ].isvalidindex( statIdx )
}



string function RankedTrials_GetDescription( ItemFlavor rankedTrial, int statIdx )
{
	Assert( ItemFlavor_GetType( rankedTrial ) == eItemType.ranked_trial )

	var settingsBlock = ItemFlavor_GetSettingsBlock( rankedTrial )
	switch ( statIdx )
	{
		case eRankedTrialGoalIdx.PRIMARY:
			return GetSettingsBlockString( settingsBlock, SETTINGS_KEY_DESCRIPTION_PRIMARY )

		case eRankedTrialGoalIdx.SECONDARY_ONE:
			return GetSettingsBlockString( settingsBlock, SETTINGS_KEY_DESCRIPTION_SECONDARY_ONE )

		case eRankedTrialGoalIdx.SECONDARY_TWO:
			return GetSettingsBlockString( settingsBlock, SETTINGS_KEY_DESCRIPTION_SECONDARY_TWO )

		default:
			Assert( false )
			return ""
	}

	unreachable
}




int function _GetPersistenceData( entity player, string persistenceValueKey )
{
	Assert( PERSISTENCE_KEYS.contains( persistenceValueKey ) )
	string platformId = GetMergedPlatformIdForPlayer( player )
	string persistenceKey = format( PERSISTENCE_KEY_FORMAT_STRING, platformId, persistenceValueKey )





	return player.GetPersistentVarAsInt( persistenceKey )
}

bool function RankedTrials_PlayerHasAssignedTrial( entity player )
{
	if ( GetConVarBool( CONVAR_KILL_SWITCH ) )
		return false

	string platformId = GetMergedPlatformIdForPlayer( player )



	int guid = player.GetPersistentVarAsInt( format( PERSISTENCE_KEY_FORMAT_STRING, platformId, PERSISTENCE_KEY_GUID ) )


	return ( IsValidItemFlavorGUID( guid, eValidation.DONT_ASSERT ) )
}

ItemFlavor function RankedTrials_GetAssignedTrial( entity player )
{
	string platformId = GetMergedPlatformIdForPlayer( player )



	int guid = player.GetPersistentVarAsInt( format( PERSISTENCE_KEY_FORMAT_STRING, platformId, PERSISTENCE_KEY_GUID ) )


	Assert( IsValidItemFlavorGUID( guid, eValidation.ASSERT ) )
	return GetItemFlavorByGUID( guid )
}

int function RankedTrials_GetNetLP( entity player )
{
	return _GetPersistenceData( player, PERSISTENCE_KEY_NET_LP_DURING_TRIAL )
}

int function RankedTrials_GetSecondaryStatMatchComboStatProgress( entity player )
{
	return _GetPersistenceData( player, PERSISTENCE_KEY_SECONDARY_STAT_PROGRESS_COMBO )
}

int function RankedTrials_GetGamesPlayedInTrialsState( entity player )
{
	return _GetPersistenceData( player, PERSISTENCE_KEY_GAMES_IN_TRIAL_STATE )
}

int function RankedTrials_GetGamesAllowedInTrialsState( entity player, ItemFlavor rankedTrial )
{
	int timesFailed  = _GetPersistenceData( player, PERSISTENCE_KEY_TIMES_FAILED_TRIAL )
	int matchCount   = RankedTrials_GetTrialMatchCount( rankedTrial )

	return minint( ( timesFailed + matchCount ), RankedTrials_GetTrialMaxMatchCount( rankedTrial ) )
}

int function RankedTrials_GetTimesFailedTrial( entity player )
{
	return _GetPersistenceData( player, PERSISTENCE_KEY_TIMES_FAILED_TRIAL )
}

int function RankedTrials_GetTrialState( entity player )
{
	int state = _GetPersistenceData( player, PERSISTENCE_KEY_TRIAL_STATE )
	Assert( state >= 0 && state < eRankedTrialState.COUNT )
	return state
}


bool function RankedTrials_PlayerHasIncompleteTrial( entity player )
{
	return RankedTrials_PlayerHasAssignedTrial( player ) && RankedTrials_GetTrialState( player ) == eRankedTrialState.INCOMPLETE
}

bool function RankedTrials_NextRankHasTrial( SharedRankedDivisionData currentDivision, SharedRankedDivisionData ornull nextDivision )
{
	if ( nextDivision != null )
	{
		expect SharedRankedDivisionData( nextDivision )
		return currentDivision.tier != nextDivision.tier
	}
	return false
}


int function RankedTrials_GetProgressValueForStatByIndex( entity player, int statIdx )
{
	switch ( statIdx )
	{
		case eRankedTrialGoalIdx.PRIMARY:
			return _GetPersistenceData( player, PERSISTENCE_KEY_PRIMARY_STAT_PROGRESS )

		case eRankedTrialGoalIdx.SECONDARY_ONE:
			return _GetPersistenceData( player, PERSISTENCE_KEY_SECONDARY_STAT_PROGRESS_ONE )

		case eRankedTrialGoalIdx.SECONDARY_TWO:
			return _GetPersistenceData( player, PERSISTENCE_KEY_SECONDARY_STAT_PROGRESS_TWO )
	}
	Assert( false )
	unreachable
}








































































































































































































































































#if DEV
bool function DEV_CheckPlaylistOverrides()
{
	if ( !GetCurrentPlaylistVarBool( DEV_RUN_PLAYLIST_OVERRIDE_CHECK, false ) )
		return true

	














	foreach ( string _, int rankedTrialGUID in file.rankedTierToTrialMap )
	{
		ItemFlavor rankedTrial = GetItemFlavorByGUID( rankedTrialGUID )
		printf( "RANKED_TRIALS_DBG: listing playlist overrides for Ranked Trial %s", ItemFlavor_GetGUIDString( rankedTrial ) )
		printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetTrialMatchCount: %d", RankedTrials_GetTrialMatchCount( rankedTrial ) )
		printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetTrialMaxMatchCount: %d", RankedTrials_GetTrialMaxMatchCount( rankedTrial ) )
		printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetTrialBonusLPGain: %d", RankedTrials_GetTrialBonusLPGain( rankedTrial ) )
		printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetTrialMaxLPLoss: %d", RankedTrials_GetTrialMaxLPLoss( rankedTrial ) )
		printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_SecondaryTrialRequiresSingleMatchPerformance: %s", RankedTrials_SecondaryTrialRequiresSingleMatchPerformance( rankedTrial ) ? "true" : "false" )
		printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetSecondaryTrialSingleMatchComboCount: %d", RankedTrials_SecondaryTrialRequiresSingleMatchPerformance( rankedTrial ) ? RankedTrials_GetSecondaryTrialSingleMatchComboCount( rankedTrial ) : -1 )

		printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetTrialStatGoalByIndex | PRIMARY: %d", RankedTrials_GetTrialStatGoalByIndex( rankedTrial, eRankedTrialGoalIdx.PRIMARY ) )
		printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetTrialStatGoalByIndex | SECONDARY_ONE: %d", RankedTrials_GetTrialStatGoalByIndex( rankedTrial, eRankedTrialGoalIdx.SECONDARY_ONE ) )
		if ( RankedTrials_HasDualStatSecondaryTrial( rankedTrial ) )
			printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetTrialStatGoalByIndex | SECONDARY_TWO: %d", RankedTrials_GetTrialStatGoalByIndex( rankedTrial, eRankedTrialGoalIdx.SECONDARY_TWO ) )

		StatEntry stat = RankedTrials_GetTrialStatEntryByIndex( rankedTrial, eRankedTrialGoalIdx.PRIMARY )
		printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetTrialStatEntryByIndex | PRIMARY: %s", stat.persistenceFullKey_Current )
		if ( RankedTrials_HasSecondaryTrial( rankedTrial ) )
		{
			stat = RankedTrials_GetTrialStatEntryByIndex( rankedTrial, eRankedTrialGoalIdx.SECONDARY_ONE )
			printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetTrialStatEntryByIndex | SECONDARY_ONE: %s", stat.persistenceFullKey_Current )
			if ( RankedTrials_HasDualStatSecondaryTrial( rankedTrial ) )
			{
				stat = RankedTrials_GetTrialStatEntryByIndex( rankedTrial, eRankedTrialGoalIdx.SECONDARY_TWO )
				printf( "RANKED_TRIALS_DBG: playlist override - RankedTrials_GetTrialStatEntryByIndex | SECONDARY_TWO: %s", stat.persistenceFullKey_Current )
			}
		}
	}

	if ( file.rankedTierToTrialMap.len() == 0 )
		printt( "RANKED_TRIALS_DBG: rankedTierToTrialMap is empty!" )

	if ( file.trialStatRefCache.len() == 0 )
		printt( "RANKED_TRIALS_DBG: trialStatRefCache is empty!" )

	return true
}



















#endif
