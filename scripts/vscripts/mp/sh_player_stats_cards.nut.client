





























global function StatCard_GetAvailableSeasons
global function StatCard_ClearAvailableSeasonsCache
global function StatCard_GetAvailableRankedPeriods
global function StatCard_ClearAvailableRankedPeriodsCache
global function StatCard_GetAvailableSeasonsAndRankedPeriods
global function StatCard_ClearAvailableSeasonsAndRankedPeriodsCache




global enum eStatCardGameMode
{
	BATTLE_ROYALE,
	ARENAS,
	_count
	UNKNOWN,
}

global enum eStatCardType
{
	CAREER,
	SEASON,
	RANKEDPERIOD,
	RANKEDCAREER,
	UNKNOWN,

	_count
}

enum eStatCardSection
{
	HEADER,
	HEADERTOOLTIP,
	BODY,
	BODYTOOLTIP,

	_count
}

enum eStatCalcMethod
{
	SIMPLE,
	CHARACTER_AGGREGATE,
	CHARACTER_HIGHEST,
	WEAPON_AGGREGATE,
	WEAPON_HIGHEST,
	MATH_ADD,
	MATH_SUB,
	MATH_MULTIPLY,
	MATH_DIVIDE,
	MATH_WINRATE,
	SEASON_CHARACTER_HIGHEST,

	_count
}

struct StatCardEntry
{
	int	   gameMode
	int    cardType
	int    section
	int    calcMethod
	string label
	string statRef
	string mathRef
}

struct StatCardStruct
{
	array<StatCardEntry> headerStats
	array<StatCardEntry> headerToolTipStats
	array<StatCardEntry> bodyStats
	array<StatCardEntry> bodyToolTipStats
}

struct LegendStatStruct
{
	string	   legendName
	asset	   portrait
	int        gamesPlayed
	float      winRate
	float      kdr
}

struct WeaponStatStruct
{
	string weaponName
	asset portrait
	int kills
	int damage
	float headshotRatio
	float accuracy
}

struct
{
	var statsRui
	StatCardStruct careerStatCard
	StatCardStruct seasonStatCard
	StatCardStruct rankedPeriodStatCard

	int selectedMode
	
	table< int, table< int, StatCardStruct > > statCards

	table<string, array<string> > toolTipStrings

	table<string, int> GUIDToSeasonNumber
	int currentGUIDToSeasonNumber = 1

	table< int, array<ItemFlavor> > availableSeasonsCache
	table< int, array<ItemFlavor> > availableRankedPeriodsCache
	table< int, array<ItemFlavor> > availableSeasonsAndRankedPeriods
	table< int, array<ItemFlavor> > currentAndLastUnrankedAndRankedSeasons
} file

const string NO_DATA_REF = "000"

const STAT_TOOLTIP_HEADER_CAREER = "careerHeader"
const STAT_TOOLTIP_LCIRCLE_CAREER = "careerLeftCircle"
const STAT_TOOLTIP_RCIRCLE_CAREER = "careerRightCircle"
const STAT_TOOLTIP_COLUMNA_CAREER = "careerColumnA"
const STAT_TOOLTIP_COLUMNB_CAREER = "careerColumnB"
const STAT_TOOLTIP_HEADER_SEASON = "seasonHeader"
const STAT_TOOLTIP_LCIRCLE_SEASON = "seasonLeftCircle"
const STAT_TOOLTIP_RCIRCLE_SEASON = "seasonRightCircle"
const STAT_TOOLTIP_COLUMNA_SEASON = "seasonColumnA"
const STAT_TOOLTIP_COLUMNB_SEASON = "seasonColumnB"

const int MAX_STATS_HEADER = 3
const int MAX_STATS_BODY = 12

const bool STAT_CARD_V2_DEBUG = false







































































































































int function SetStatCalcMethodFromDataTable( string method )
{
	switch ( method.toupper() )
	{
		case "SIMPLE":
			return eStatCalcMethod.SIMPLE
		case "CHARACTER_AGGREGATE":
			return eStatCalcMethod.CHARACTER_AGGREGATE
		case "CHARACTER_HIGHEST":
			return eStatCalcMethod.CHARACTER_HIGHEST
		case "WEAPON_AGGREGATE":
			return eStatCalcMethod.WEAPON_AGGREGATE
		case "WEAPON_HIGHEST":
			return eStatCalcMethod.WEAPON_HIGHEST
		case "MATH_ADD":
			return eStatCalcMethod.MATH_ADD
		case "MATH_SUB":
			return eStatCalcMethod.MATH_SUB
		case "MATH_MULTIPLY":
			return eStatCalcMethod.MATH_MULTIPLY
		case "MATH_DIVIDE":
			return eStatCalcMethod.MATH_DIVIDE
		case "MATH_WINRATE":
			return eStatCalcMethod.MATH_WINRATE
		case "SEASON_CHARACTER_HIGHEST":
			return eStatCalcMethod.SEASON_CHARACTER_HIGHEST
		default:
			Assert( false, format("Stat Card: Unknown Stat Card Calc Type Provided (%s)", method) )
	}

	unreachable
}

















const string STATCARD_VAR_FORMAT_HEADER_LABEL = "headerStatFieldLabel"
const string STATCARD_VAR_FORMAT_HEADER_DISPLAY = "headerStatFieldDisplay"
const string STATCARD_VAR_FORMAT_LABEL = "statFieldLabel"
const string STATCARD_VAR_FORMAT_DISPLAY = "statFieldDisplay"

const string STATCARD_VAR_FORMAT_HEADER_LABEL_SEASON = "seasonHeaderStatLabel"
const string STATCARD_VAR_FORMAT_HEADER_DISPLAY_SEASON = "seasonHeaderStatDisplay"

const string STATCARD_CAREER_STAT_LABEL = "careerStatLabel"
const string STATCARD_CAREER_STAT_DISPLAY = "careerStatDisplay"
const string STATCARD_SEASON_STAT_LABEL = "seasonStatLabel"
const string STATCARD_SEASON_STAT_DISPLAY = "seasonStatDisplay"






























































































































































































































































































































































































string function GetDataForStat( entity player, string statRef, string mathRef, int calcMethod, string modeRef = "", string seasonOrRankedRef = "" )
{
	if( statRef == NO_DATA_REF )
	{
		return statRef
	}
	else
	{
		StatTemplate stat = GetStatTemplateFromString( statRef )

		bool statComesFromAggregate = (calcMethod == eStatCalcMethod.CHARACTER_AGGREGATE) || (calcMethod == eStatCalcMethod.CHARACTER_HIGHEST) || (calcMethod == eStatCalcMethod.WEAPON_AGGREGATE) || (calcMethod == eStatCalcMethod.WEAPON_HIGHEST) || (calcMethod == eStatCalcMethod.SEASON_CHARACTER_HIGHEST)
		bool statComesFromMath = (calcMethod == eStatCalcMethod.MATH_DIVIDE) || (calcMethod == eStatCalcMethod.MATH_MULTIPLY) || (calcMethod == eStatCalcMethod.MATH_SUB) || (calcMethod == eStatCalcMethod.MATH_ADD) || (calcMethod == eStatCalcMethod.MATH_WINRATE)

		int data
		if ( calcMethod == eStatCalcMethod.SIMPLE )
		{
			if( modeRef == "" )
			{
				if ( seasonOrRankedRef == "" )
					data = GetStat_Int( player, ResolveStatEntry( stat ), eStatGetWhen.CURRENT )
				else
					data = GetStat_Int( player, ResolveStatEntry( stat, seasonOrRankedRef ), eStatGetWhen.CURRENT )
			}
			else
			{
				if ( seasonOrRankedRef == "" )
					data = GetStat_Int( player, ResolveStatEntry( stat, modeRef ), eStatGetWhen.CURRENT )
				else
					data = GetStat_Int( player, ResolveStatEntry( stat, modeRef, seasonOrRankedRef ), eStatGetWhen.CURRENT )
			}
		}
		else if ( statComesFromAggregate )
		{
			data = AggregateStat( player, stat, calcMethod, seasonOrRankedRef )
		}
		else if ( statComesFromMath )
		{
			Assert( (mathRef != ""), format( "Stat Cards: Attempted to calculate a stat value without providing two stats to calculate from (%s)", mathRef) )

			StatTemplate mathStat = GetStatTemplateFromString( mathRef )
			float calcData = CalculateStat( player, stat, mathStat, calcMethod, seasonOrRankedRef, "" )
			float modValue = (calcData % 1) * 100.0

			string result
			if ( modValue != 0 )
				result = format( "%02d.%02d", calcData, modValue )
			else
				result = string( calcData )
			return result
		}

		return format( "%02d", data )
	}

	unreachable
}

float function GetDataForStat_Float( entity player, string statRef, string mathRef, int calcMethod, string modeRef, string seasonOrRankedRef = "" )
{
	if( statRef == NO_DATA_REF )
	{
		
		return -1
	}
	else
	{
#if STAT_CARD_V2_DEBUG
			printf( "StatCardV2Debug: Collecting Data for %s", statRef )
#endif

		StatTemplate stat = GetStatTemplateFromString( statRef )

		bool statComesFromAggregate = (calcMethod == eStatCalcMethod.CHARACTER_AGGREGATE) || (calcMethod == eStatCalcMethod.CHARACTER_HIGHEST) || (calcMethod == eStatCalcMethod.WEAPON_AGGREGATE) || (calcMethod == eStatCalcMethod.WEAPON_HIGHEST) || (calcMethod == eStatCalcMethod.SEASON_CHARACTER_HIGHEST)
		bool statComesFromMath = (calcMethod == eStatCalcMethod.MATH_DIVIDE) || (calcMethod == eStatCalcMethod.MATH_MULTIPLY) || (calcMethod == eStatCalcMethod.MATH_SUB) || (calcMethod == eStatCalcMethod.MATH_ADD) || (calcMethod == eStatCalcMethod.MATH_WINRATE)

		int data
		if ( calcMethod == eStatCalcMethod.SIMPLE )
		{
			if( modeRef == "" || !ShouldIncludeModeRef( modeRef, seasonOrRankedRef ) )
			{
				if ( seasonOrRankedRef == "" )
					data = GetStat_Int( player, ResolveStatEntry( stat ), eStatGetWhen.CURRENT )
				else
					data = GetStat_Int( player, ResolveStatEntry( stat, seasonOrRankedRef ), eStatGetWhen.CURRENT )
			}
			else
			{
				if ( seasonOrRankedRef == "" )
					data = GetStat_Int( player, ResolveStatEntry( stat, modeRef ), eStatGetWhen.CURRENT )
				else
					data = GetStat_Int( player, ResolveStatEntry( stat, modeRef, seasonOrRankedRef ), eStatGetWhen.CURRENT )
			}
		}
		else if ( statComesFromAggregate )
		{
			data = AggregateStat( player, stat, calcMethod, modeRef, seasonOrRankedRef )
		}
		else if ( statComesFromMath )
		{
			Assert( (mathRef != ""), format( "Stat Cards: Attempted to calculate a stat value without providing two stats to calculate from (%s)", mathRef) )

			StatTemplate mathStat = GetStatTemplateFromString( mathRef )
			float calcData = CalculateStat( player, stat, mathStat, calcMethod, modeRef, seasonOrRankedRef, "" )

			return calcData
		}

		return float( data )
	}

	unreachable
}

StatTemplate function GetStatTemplateFromString( string statRef )
{
	switch ( statRef )
	{
		
		case "CAREER_STATS.games_played":
			return CAREER_STATS.games_played
		case "CAREER_STATS.placements_win":
			return CAREER_STATS.placements_win
		case "CAREER_STATS.placements_top_3":
			return CAREER_STATS.placements_top_3
		case "CAREER_STATS.kills":
			return CAREER_STATS.kills
		case "CAREER_STATS.deaths":
			return CAREER_STATS.deaths
		case "CAREER_STATS.dooms":
			return CAREER_STATS.dooms
		case "CAREER_STATS.team_work_kill_count":
			return CAREER_STATS.team_work_kill_count
		case "CAREER_STATS.revived_ally":
			return CAREER_STATS.revived_ally
		case "CAREER_STATS.damage_done":
			return CAREER_STATS.damage_done
		case "CAREER_STATS.character_damage_done_max_single_game":
			return CAREER_STATS.character_damage_done_max_single_game
		case "CAREER_STATS.season_character_kills":
			return CAREER_STATS.season_character_kills
		case "CAREER_STATS.season_character_damage_done":
			return CAREER_STATS.season_character_damage_done
		case "CAREER_STATS.season_character_placements_win":
			return CAREER_STATS.season_character_placements_win
		case "CAREER_STATS.season_character_placements_top_5":
			return CAREER_STATS.season_character_placements_top_5
		case "CAREER_STATS.weapon_damage_done":
			return CAREER_STATS.weapon_damage_done
		case "CAREER_STATS.weapon_headshots":
			return CAREER_STATS.weapon_headshots
		case "CAREER_STATS.weapon_shots":
			return CAREER_STATS.weapon_shots
		case "CAREER_STATS.weapon_hits":
			return CAREER_STATS.weapon_hits
		case "CAREER_STATS.character_games_played":
			return CAREER_STATS.character_games_played
		case "CAREER_STATS.character_kills":
			return CAREER_STATS.character_kills
		case "CAREER_STATS.character_deaths":
			return CAREER_STATS.character_deaths
		case "CAREER_STATS.character_placements_win":
			return CAREER_STATS.character_placements_win
		case "CAREER_STATS.times_respawned_ally":
			return CAREER_STATS.times_respawned_ally

		
		case "CAREER_STATS.season_games_played":
			return CAREER_STATS.season_games_played
		case "CAREER_STATS.season_damage_done":
			return CAREER_STATS.season_damage_done
		case "CAREER_STATS.season_kills":
			return CAREER_STATS.season_kills
		case "CAREER_STATS.season_deaths":
			return CAREER_STATS.season_deaths
		case "CAREER_STATS.season_dooms":
			return CAREER_STATS.season_dooms
		case "CAREER_STATS.season_team_work_kill_count":
			return CAREER_STATS.season_team_work_kill_count
		case "CAREER_STATS.season_revived_ally":
			return CAREER_STATS.season_revived_ally
		case "CAREER_STATS.season_times_respawned_ally":
			return CAREER_STATS.season_times_respawned_ally
		case "CAREER_STATS.season_character_damage_done_max_single_game":
			return CAREER_STATS.season_character_damage_done_max_single_game
		case "CAREER_STATS.assists":
			return CAREER_STATS.assists
		case "CAREER_STATS.season_assists":
			return CAREER_STATS.season_assists
		case "CAREER_STATS.kills_max_single_game":
			return CAREER_STATS.kills_max_single_game
		case "CAREER_STATS.season_kills_max_single_game":
			return CAREER_STATS.season_kills_max_single_game
		case "CAREER_STATS.win_streak_longest":
			return CAREER_STATS.win_streak_longest
		case "CAREER_STATS.season_win_streak_longest":
			return CAREER_STATS.season_win_streak_longest
		case "CAREER_STATS.season_placements_win":
			return CAREER_STATS.season_placements_win
		case "CAREER_STATS.placements_top_5":
			return CAREER_STATS.placements_top_5
		case "CAREER_STATS.placements_top_10":
			return CAREER_STATS.placements_top_10

		
		case "CAREER_STATS.rankedperiod_assists":
			return CAREER_STATS.rankedperiod_assists
		case "CAREER_STATS.rankedperiod_character_damage_done_max_single_game":
			return CAREER_STATS.rankedperiod_character_damage_done_max_single_game
		case "CAREER_STATS.rankedperiod_damage_done":
			return CAREER_STATS.rankedperiod_damage_done
		case "CAREER_STATS.rankedperiod_deaths":
			return CAREER_STATS.rankedperiod_deaths
		case "CAREER_STATS.rankedperiod_dooms":
			return CAREER_STATS.rankedperiod_dooms
		case "CAREER_STATS.rankedperiod_games_played":
			return CAREER_STATS.rankedperiod_games_played
		case "CAREER_STATS.rankedperiod_kills":
			return CAREER_STATS.rankedperiod_kills
		case "CAREER_STATS.rankedperiod_kills_max_single_game":
			return CAREER_STATS.rankedperiod_kills_max_single_game
		case "CAREER_STATS.rankedperiod_placements_top_5":
			return CAREER_STATS.rankedperiod_placements_top_5
		case "CAREER_STATS.rankedperiod_placements_win":
			return CAREER_STATS.rankedperiod_placements_win
		case "CAREER_STATS.rankedperiod_revived_ally":
			return CAREER_STATS.rankedperiod_revived_ally
		case "CAREER_STATS.rankedperiod_times_respawned_ally":
			return CAREER_STATS.rankedperiod_times_respawned_ally
		case "CAREER_STATS.rankedperiod_win_streak_longest":
			return CAREER_STATS.rankedperiod_win_streak_longest

		
		case "CAREER_STATS.modes_games_played":
			return CAREER_STATS.modes_games_played
		case "CAREER_STATS.modes_placements_win":
			return CAREER_STATS.modes_placements_win
		case "CAREER_STATS.modes_damage_done":
			return CAREER_STATS.modes_damage_done
		case "CAREER_STATS.modes_damage_done_max_single_game":
			return CAREER_STATS.modes_damage_done_max_single_game
		case "CAREER_STATS.modes_kills":
			return CAREER_STATS.modes_kills
		case "CAREER_STATS.modes_deaths":
			return CAREER_STATS.modes_deaths
		case "CAREER_STATS.modes_kills_max_single_game":
			return CAREER_STATS.modes_kills_max_single_game
		case "CAREER_STATS.modes_dooms":
			return CAREER_STATS.modes_dooms
		case "CAREER_STATS.modes_assists":
			return CAREER_STATS.modes_assists
		case "CAREER_STATS.modes_win_streak_longest":
			return CAREER_STATS.modes_win_streak_longest
		case "CAREER_STATS.modes_revived_ally":
			return CAREER_STATS.modes_revived_ally

		
		case "CAREER_STATS.modes_season_games_played":
			return CAREER_STATS.modes_season_games_played
		case "CAREER_STATS.modes_season_placements_win":
			return CAREER_STATS.modes_season_placements_win
		case "CAREER_STATS.modes_season_damage_done":
			return CAREER_STATS.modes_season_damage_done
		case "CAREER_STATS.modes_season_damage_done_max_single_game":
			return CAREER_STATS.modes_season_damage_done_max_single_game
		case "CAREER_STATS.modes_season_kills":
			return CAREER_STATS.modes_season_kills
		case "CAREER_STATS.modes_season_kills_max_single_game":
			return CAREER_STATS.modes_season_kills_max_single_game
		case "CAREER_STATS.modes_season_deaths":
			return CAREER_STATS.modes_season_deaths
		case "CAREER_STATS.modes_season_dooms":
			return CAREER_STATS.modes_season_dooms
		case "CAREER_STATS.modes_season_assists":
			return CAREER_STATS.modes_season_assists
		case "CAREER_STATS.modes_season_win_streak_current":
			return CAREER_STATS.modes_season_win_streak_current
		case "CAREER_STATS.modes_season_win_streak_longest":
			return CAREER_STATS.modes_season_win_streak_longest
		case "CAREER_STATS.modes_season_revived_ally":
			return CAREER_STATS.modes_season_revived_ally

		
		case "CAREER_STATS.arenas_rankedperiod_games_played":
			return CAREER_STATS.arenas_rankedperiod_games_played
		case "CAREER_STATS.arenas_rankedperiod_placements_win":
			return CAREER_STATS.arenas_rankedperiod_placements_win
		case "CAREER_STATS.arenas_rankedperiod_damage_done":
			return CAREER_STATS.arenas_rankedperiod_damage_done
		case "CAREER_STATS.arenas_rankedperiod_damage_done_max_single_game":
			return CAREER_STATS.arenas_rankedperiod_damage_done_max_single_game
		case "CAREER_STATS.arenas_rankedperiod_damage_done":
			return CAREER_STATS.arenas_rankedperiod_damage_done
		case "CAREER_STATS.arenas_rankedperiod_kills":
			return CAREER_STATS.arenas_rankedperiod_kills
		case "CAREER_STATS.arenas_rankedperiod_kills_max_single_game":
			return CAREER_STATS.arenas_rankedperiod_kills_max_single_game
		case "CAREER_STATS.arenas_rankedperiod_deaths":
			return CAREER_STATS.arenas_rankedperiod_deaths
		case "CAREER_STATS.arenas_rankedperiod_dooms":
			return CAREER_STATS.arenas_rankedperiod_dooms
		case "CAREER_STATS.arenas_rankedperiod_assists":
			return CAREER_STATS.arenas_rankedperiod_assists
		case "CAREER_STATS.arenas_rankedperiod_win_streak_current_new":
			return CAREER_STATS.arenas_rankedperiod_win_streak_current_new
		case "CAREER_STATS.arenas_rankedperiod_win_streak_longest_new":
			return CAREER_STATS.arenas_rankedperiod_win_streak_longest_new
		case "CAREER_STATS.arenas_rankedperiod_revived_ally":
			return CAREER_STATS.arenas_rankedperiod_revived_ally

		case "CAREER_STATS.arenas_rankedcareer_games_played":
			return CAREER_STATS.arenas_rankedcareer_games_played
		case "CAREER_STATS.arenas_rankedcareer_placements_win":
			return CAREER_STATS.arenas_rankedcareer_placements_win
		case "CAREER_STATS.arenas_rankedcareer_placements_win":
			return CAREER_STATS.arenas_rankedcareer_placements_win
		case "CAREER_STATS.arenas_rankedcareer_damage_done":
			return CAREER_STATS.arenas_rankedcareer_damage_done
		case "CAREER_STATS.arenas_rankedcareer_damage_done_max_single_game":
			return CAREER_STATS.arenas_rankedcareer_damage_done_max_single_game
		case "CAREER_STATS.arenas_rankedcareer_damage_done":
			return CAREER_STATS.arenas_rankedcareer_damage_done
		case "CAREER_STATS.arenas_rankedcareer_kills":
			return CAREER_STATS.arenas_rankedcareer_kills
		case "CAREER_STATS.arenas_rankedcareer_deaths":
			return CAREER_STATS.arenas_rankedcareer_deaths
		case "CAREER_STATS.arenas_rankedcareer_kills":
			return CAREER_STATS.arenas_rankedcareer_kills
		case "CAREER_STATS.arenas_rankedcareer_kills_max_single_game":
			return CAREER_STATS.arenas_rankedcareer_kills_max_single_game
		case "CAREER_STATS.arenas_rankedcareer_dooms":
			return CAREER_STATS.arenas_rankedcareer_dooms
		case "CAREER_STATS.arenas_rankedcareer_assists":
			return CAREER_STATS.arenas_rankedcareer_assists
		case "CAREER_STATS.arenas_rankedcareer_win_streak_longest":
			return CAREER_STATS.arenas_rankedcareer_win_streak_longest
		case "CAREER_STATS.arenas_rankedcareer_revived_ally":
			return CAREER_STATS.arenas_rankedcareer_revived_ally

		default:
			Assert( false, format( "Stat Card attempted to look up an unknown StatTemplate: %s", statRef) )
	}

	unreachable
}

int function AggregateStat( entity player, StatTemplate stat, int calcMethod, string modeRef, string seasonOrRankedRef = "" )
{
	int total

	if ( calcMethod == eStatCalcMethod.CHARACTER_HIGHEST || calcMethod == eStatCalcMethod.CHARACTER_AGGREGATE )
	{
		foreach( characterRef in GetAllCharacterGUIDStringsForStats() )
			total = AggregateStatInternal( total, characterRef, player, stat, calcMethod, modeRef, seasonOrRankedRef )
	}
	else if ( calcMethod == eStatCalcMethod.SEASON_CHARACTER_HIGHEST )
	{
		foreach ( ItemFlavor season in GetAllSeasonFlavors() )
			foreach( characterRef in GetAllCharacterGUIDStringsForStats() )
				total = AggregateStatInternal( total, characterRef, player, stat, calcMethod, modeRef, ItemFlavor_GetGUIDString( season ) )
	}

	return total
}

int function AggregateStatInternal( int total, string characterRef, entity player, StatTemplate stat, int calcMethod, string modeRef, string seasonOrRankedRef = "" )
{
	
	int statValue
	if ( modeRef == "" || !ShouldIncludeModeRef( modeRef, seasonOrRankedRef ) )
	{
		if ( seasonOrRankedRef == "" )
			statValue = GetStat_Int( player, ResolveStatEntry( stat, characterRef ), eStatGetWhen.CURRENT )
		else
			statValue = GetStat_Int( player, ResolveStatEntry( stat, seasonOrRankedRef, characterRef ), eStatGetWhen.CURRENT )
	}
	else
	{
		if ( seasonOrRankedRef == "" )
			statValue = GetStat_Int( player, ResolveStatEntry( stat, modeRef, characterRef ), eStatGetWhen.CURRENT )
		else
			statValue = GetStat_Int( player, ResolveStatEntry( stat, modeRef, seasonOrRankedRef, characterRef ), eStatGetWhen.CURRENT )
	}

	if ( (calcMethod == eStatCalcMethod.CHARACTER_HIGHEST || calcMethod == eStatCalcMethod.SEASON_CHARACTER_HIGHEST) && (statValue > total) )
		total = statValue
	if ( calcMethod == eStatCalcMethod.CHARACTER_AGGREGATE )
		total += statValue
	return total
}

float function CalculateStat( entity player, StatTemplate stat1, StatTemplate stat2, int calcMethod, string modeRef, string seasonOrRankedRef = "", string ref = "" )
{
	int stat1Int
	int stat2Int

	if ( seasonOrRankedRef == "" )
	{
		if ( modeRef == "" || !ShouldIncludeModeRef( modeRef, seasonOrRankedRef ) )
		{
			stat1Int = GetStat_Int( player, ResolveStatEntry( stat1 ), eStatGetWhen.CURRENT )
			stat2Int = GetStat_Int( player, ResolveStatEntry( stat2 ), eStatGetWhen.CURRENT )
		}
		else
		{
			stat1Int = GetStat_Int( player, ResolveStatEntry( stat1, modeRef ), eStatGetWhen.CURRENT )
			stat2Int = GetStat_Int( player, ResolveStatEntry( stat2, modeRef ), eStatGetWhen.CURRENT )
		}
	}
	else if ( ref != "" )
	{
		if ( modeRef == "" || !ShouldIncludeModeRef( modeRef, seasonOrRankedRef ) )
		{
			stat1Int = GetStat_Int( player, ResolveStatEntry( stat1, ref ), eStatGetWhen.CURRENT )
			stat2Int = GetStat_Int( player, ResolveStatEntry( stat2, ref ), eStatGetWhen.CURRENT )
		}
		else
		{
			stat1Int = GetStat_Int( player, ResolveStatEntry( stat1, modeRef, ref ), eStatGetWhen.CURRENT )
			stat2Int = GetStat_Int( player, ResolveStatEntry( stat2, modeRef, ref ), eStatGetWhen.CURRENT )
		}
	}
	else
	{
		if ( modeRef == "" || !ShouldIncludeModeRef( modeRef, seasonOrRankedRef ) )
		{
			stat1Int = GetStat_Int( player, ResolveStatEntry( stat1, seasonOrRankedRef ), eStatGetWhen.CURRENT )
			stat2Int = GetStat_Int( player, ResolveStatEntry( stat2, seasonOrRankedRef ), eStatGetWhen.CURRENT )
		}
		else
		{
			stat1Int = GetStat_Int( player, ResolveStatEntry( stat1, modeRef, seasonOrRankedRef ), eStatGetWhen.CURRENT )
			stat2Int = GetStat_Int( player, ResolveStatEntry( stat2, modeRef, seasonOrRankedRef ), eStatGetWhen.CURRENT )
		}
	}

	switch ( calcMethod )
	{
		case eStatCalcMethod.MATH_ADD:
			return float( stat1Int + stat2Int )
		case eStatCalcMethod.MATH_SUB:
			return float( stat1Int - stat2Int )
		case eStatCalcMethod.MATH_MULTIPLY:
			return float( stat1Int * stat2Int )
	}

	if ( calcMethod == eStatCalcMethod.MATH_DIVIDE )
	{
		if ( stat2Int != 0 )
			printf( "StatMathDebug: %i / %i = %f", stat1Int, stat2Int, (float(stat1Int)/float(stat2Int)) )

		if ( stat2Int != 0 )
			return float( stat1Int ) / float( stat2Int )
		else
			return float( stat1Int )
	}

	if ( calcMethod == eStatCalcMethod.MATH_WINRATE )
	{
		if ( stat2Int != 0 )
			return (float( stat1Int ) / float( stat2Int )) * 100.0
		else
			return 0
	}

	unreachable
}

void function StatCard_ClearAvailableSeasonsCache( int gameMode )
{
	if ( gameMode in file.availableSeasonsCache )
		delete file.availableSeasonsCache[gameMode]
}

array<ItemFlavor> function StatCard_GetAvailableSeasons( int gameMode )
{
	if ( gameMode in file.availableSeasonsCache )
		return clone file.availableSeasonsCache[gameMode]

	array<ItemFlavor> seasons = clone GetAllSeasonFlavors()

	foreach( ItemFlavor season in seasons )
	{
		if ( !CalEvent_IsRevealed( season, GetUnixTimestamp() ) )
			seasons.removebyvalue( season )

		
		if ( gameMode == eStatCardGameMode.ARENAS )
		{
			ItemFlavor season09CalEvent = GetItemFlavorByAsset( $"settings/itemflav/calevent/season09.rpak" )
			int season09StartTime       = CalEvent_GetStartUnixTime( season09CalEvent )
			int startTime               = CalEvent_GetStartUnixTime( season )
			if ( startTime < season09StartTime )
				seasons.removebyvalue( season )

			ItemFlavor season15CalEvent = GetItemFlavorByAsset( $"settings/itemflav/calevent/season15.rpak" ) 
			int season15EndTime = CalEvent_GetFinishUnixTime( season15CalEvent )
			if ( startTime >= season15EndTime )
				seasons.removebyvalue( season )
		}
	}

	file.availableSeasonsCache[gameMode] <- seasons
	return seasons
}

void function StatCard_ClearAvailableRankedPeriodsCache( int gameMode )
{
	if ( gameMode in file.availableRankedPeriodsCache )
		delete file.availableRankedPeriodsCache[gameMode]
}

array<ItemFlavor> function StatCard_GetAvailableRankedPeriods( int gameMode )
{
	if ( gameMode in file.availableRankedPeriodsCache )
		return clone file.availableRankedPeriodsCache[gameMode]


		ItemFlavor season09CalEvent = GetItemFlavorByAsset( $"settings/itemflav/calevent/season09.rpak" )


	array<ItemFlavor> brRankedPeriods = GetAllRankedPeriodCalEventFlavorsByType( eItemType.calevent_rankedperiod )

		array<ItemFlavor> arenaRankedPeriods = GetAllRankedPeriodCalEventFlavorsByType( eItemType.calevent_arenas_ranked_period )


	foreach ( ItemFlavor period in brRankedPeriods )
	{
		if ( !CalEvent_IsRevealed( period, GetUnixTimestamp() ) )
			brRankedPeriods.removebyvalue( period )
	}

	array<ItemFlavor> rankedPeriods = []

		if ( gameMode == eStatCardGameMode.ARENAS )
		{
			rankedPeriods.extend( arenaRankedPeriods )
		}
		else
		{
			rankedPeriods.extend( brRankedPeriods )
		}





		if ( gameMode != eStatCardGameMode.ARENAS )
		{
			rankedPeriods.extend( Ranked_GetAllRanked2Pt0Periods() )
		}




	rankedPeriods.sort( SortSeasonAndRankedStats )


		if ( gameMode == eStatCardGameMode.ARENAS )
		{
			int season09Idx = rankedPeriods.find( season09CalEvent )
			for ( int i = 0; i < season09Idx; i++ )
			{
				rankedPeriods.remove( 0 )
			}
		}


	file.availableRankedPeriodsCache[gameMode] <- rankedPeriods
	return rankedPeriods
}

void function StatCard_ClearAvailableSeasonsAndRankedPeriodsCache( int gameMode )
{
	if ( gameMode in file.availableSeasonsAndRankedPeriods )
		delete file.availableSeasonsAndRankedPeriods[gameMode]
}

const asset SEASON_15 = $"settings/itemflav/calevent/season15.rpak" 
const string SEASON_ONE_GUID = "SAID01769158912"
array< ItemFlavor > function StatCard_GetAvailableSeasonsAndRankedPeriods( int gameMode )
{
	if ( gameMode in file.availableSeasonsAndRankedPeriods )
		return clone file.availableSeasonsAndRankedPeriods[gameMode]

	
	array< ItemFlavor > seasons = clone GetAllItemFlavorsOfType( eItemType.calevent_season ) 
	ItemFlavor season15CalEvent = GetItemFlavorByAsset( SEASON_15 )
	int season15EndTime = CalEvent_GetFinishUnixTime( season15CalEvent )

	array< ItemFlavor > seasonsCopy = clone seasons
	foreach ( ItemFlavor season in seasonsCopy )
	{
		if ( !CalEvent_IsRevealed( season, GetUnixTimestamp() ) )
		{
			seasons.removebyvalue( season )
			continue
		}

		
		string guid = ItemFlavor_GetGUIDString( season )
		if ( guid == SEASON_ONE_GUID )
		{
			seasons.removebyvalue( season )
			continue
		}

		int startTime = CalEvent_GetStartUnixTime( season )
		if ( gameMode == eStatCardGameMode.ARENAS && startTime >= season15EndTime )
		{
			seasons.removebyvalue( season )
			continue
		}
	}

	array< ItemFlavor > rankedPeriods
	rankedPeriods.extend( GetAllRankedPeriodCalEventFlavorsByType( eItemType.calevent_rankedperiod ) )


		array< ItemFlavor > arenaRankedPeriods
		arenaRankedPeriods.extend( GetAllRankedPeriodCalEventFlavorsByType( eItemType.calevent_arenas_ranked_period ) )


	foreach( ItemFlavor period in rankedPeriods )
	{
		if ( !CalEvent_IsRevealed( period, GetUnixTimestamp() ) )
			rankedPeriods.removebyvalue( period )
	}

	array< ItemFlavor > seasonsAndPeriods = []
	seasonsAndPeriods.extend( seasons )

		if ( gameMode == eStatCardGameMode.ARENAS )
		{
			seasonsAndPeriods.extend( arenaRankedPeriods )
		}
		else
		{
			seasonsAndPeriods.extend( rankedPeriods )
		}





		if ( gameMode != eStatCardGameMode.ARENAS )
		{
			seasonsAndPeriods.extend( Ranked_GetAllRanked2Pt0Periods() )
		}




	seasonsAndPeriods.sort( SortSeasonAndRankedStats )


	ItemFlavor season09CalEvent = GetItemFlavorByAsset( $"settings/itemflav/calevent/season09.rpak" )

	if ( gameMode == eStatCardGameMode.ARENAS )
	{
		int season09Idx = seasonsAndPeriods.find( season09CalEvent )
		for ( int i = 0; i < season09Idx; i++ )
		{
			seasonsAndPeriods.remove(0)
		}
	}


	file.availableSeasonsAndRankedPeriods[gameMode] <- seasonsAndPeriods
	return seasonsAndPeriods
}

int function SortSeasonAndRankedStats( ItemFlavor a, ItemFlavor b )
{
	ItemFlavor compA = a
	ItemFlavor compB = b

	if ( ItemFlavor_GetType( a ) == eItemType.ranked_2pt0_period )
		compA = Ranked_GetSeasonForRanked2Pt0Period( a )

	if ( ItemFlavor_GetType( b ) == eItemType.ranked_2pt0_period )
		compB = Ranked_GetSeasonForRanked2Pt0Period( b )

	int aTime = CalEvent_GetStartUnixTime( compA )
	int bTime = CalEvent_GetStartUnixTime( compB )

	if ( aTime < bTime )
		return -1
	else if ( aTime > bTime )
		return 1

	if ( IsSeasonFlavor( a ) && !IsSeasonFlavor( b )  )
		return -1
	else if ( !IsSeasonFlavor( a ) && IsSeasonFlavor( b ) )
		return 1

	return 0
}

string function StatCard_GetToolTipFieldFromIndex( int index, int statCardType, int statCardSection )
{
	if ( statCardType == eStatCardType.CAREER )
	{
		if ( statCardSection == eStatCardSection.HEADER )
		{
			return STAT_TOOLTIP_HEADER_CAREER
		}
		else
		{
			if ( index <= 2 )
				return STAT_TOOLTIP_LCIRCLE_CAREER
			else if ( index <= 5 )
				return STAT_TOOLTIP_RCIRCLE_CAREER
			else if ( index <= 8 )
				return STAT_TOOLTIP_COLUMNA_CAREER
			else
				return STAT_TOOLTIP_COLUMNB_CAREER
		}
	}
	else
	{
		if ( statCardSection == eStatCardSection.HEADER )
		{
			return STAT_TOOLTIP_HEADER_SEASON
		}
		else
		{
			if ( index <= 2 )
				return STAT_TOOLTIP_LCIRCLE_SEASON
			else if ( index <= 5 )
				return STAT_TOOLTIP_RCIRCLE_SEASON
			else if ( index <= 8 )
				return STAT_TOOLTIP_COLUMNA_SEASON
			else
				return STAT_TOOLTIP_COLUMNB_SEASON
		}
	}

	return ""
}


bool function ShouldIncludeModeRef( string modeRef, string seasonOrRankedRef )
{
	bool shouldInclude = true


		if ( modeRef.toupper() == "ARENAS" )
		{
			if ( seasonOrRankedRef != "" )
			{
				
				array<ItemFlavor> allRankedArenaPeriods = GetAllRankedPeriodCalEventFlavorsByType( eItemType.calevent_arenas_ranked_period )
				foreach ( ItemFlavor rankedArenaPeriod in allRankedArenaPeriods )
				{
					if ( ItemFlavor_GetGUIDString( rankedArenaPeriod ) == seasonOrRankedRef )
					{
						shouldInclude = false
						break
					}
				}
			}
		}


	return shouldInclude
}


















































































































































































































































































































































































































































































