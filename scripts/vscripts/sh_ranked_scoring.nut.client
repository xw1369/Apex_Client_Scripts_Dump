
global function Sh_RankedScoring_Init
global function Ranked_GetPointsForPlacement
global function Ranked_GetPenaltyPointsForAbandon
global function Ranked_GetParticipationMutlipler
global function Ranked_GetNumProvisionalMatchesCompleted
global function Ranked_GetNumProvisionalMatchesRequired
global function Ranked_HasCompletedProvisionalMatches

global function RankedGetPointsForKillsByPlacement
global function Ranked_GetPlayerTop5StreakCount
global function Ranked_GetTop5StreakBonusValue
global function Ranked_GetTop5StreakMax
global function Ranked_GetSoftKillCapMod
global function Ranked_GetSoftKillCapStartCount
global function Ranked_GetFirstKillBonusMod
global function Ranked_GetDemoCrewBonus
global function Ranked_GetDemoCrewPlacement
global function Ranked_GetDemoCrewKills
global function Ranked_GetLivingLegendBonus
global function Ranked_GetTop5StreakBonusValueFromStreak

global function Ranked_GetHighSkillKillBonusValue
global function Ranked_GetValueOfFirstKillBonus
global function Ranked_GetPlayerFirstKillBonus
global function Ranked_GetCombatBonusTotal
global function Ranked_GetProvisionalMatchResult















global function GetHighEndLostMultiplier

global const RANKED_LP_PROMOTION_BONUS = 250
global const RANKED_LP_DEMOTION_PENALITY = 150
global const float PARTICIPATION_MODIFIER = 0.5
global const int RANKED_NUM_PROVISIONAL_MATCHES = 10
global const float HIGH_END_LOST_MULTIPLIER = 1.5


global const int     RANKED_BONUS_TOP5STREAK = 10
global const int 	 RANKED_BONUS_TOP5STREAK_MAX = 5
global const float   RANKED_BONUS_PER_HIGH_SKILL_KILL_VALUE = 0.5

global const float   RANKED_BONUS_KILL_SOFT_CAP_MOD = 0.5
global const int     RANKED_BONUS_KILL_SOFT_CAP_COUNT_START = 6
global const float   RANKED_BONUS_FIRST_KILL_MOD = 1.0
global const int     RANKED_BONUS_DEMO_CREW_BONUS = 50
global const int     RANKED_BONUS_DEMO_CREW_PLACEMENT = 10
global const int     RANKED_BONUS_DEMO_CREW_KILLS = 10


global struct RankLadderPointsBreakdown
{
	
	

	bool isHighTier
	bool wasInProvisonalGame			
	bool wasAbandoned					
	bool lossForgiveness				
	int  damage							

	int  knockdown						
	int  knockdownAssist				

	int kills							
	int assists							
	int participation					

	int killsUnique = 0					
	int assistUnique = 0				
	int participationUnique = 0			

	int placement						
	int totalSquads						
	int placementScore					











	
	

	int killBonus = 0                      
	int convergenceBonus = 0               
	int skillDiffBonus = 0                 
	int provisionalMatchBonus = 0               
	int promotionBonus = 0							

	int demotionPenality = 0						
	int penaltyPointsForAbandoning = 0				
	int demotionProtectionAdjustment = 0			
	int lossProtectionAdjustment = 0				

	int highEndAdjustment = 0    

	
	int startingLP
	int netLP
	int finalLP


		int entryCost = 0
		int totalUniqueSquadKills = 0

		int top5Streak = 0
		int top5StreakBonusValue = 0

		int highSkillKill = 0
		int highSkillKillBonusValue = 0

		int	killValueModifierByPlacement = 0
		int firstKillBonus = 0

		int demoCrewBonus = 0


	int trialState = 0 
}

global struct RankedPlacementScoreStruct
{
	
	int   placementPosition
	int   placementPoints
	int   pointsPerKill
}

struct
{
	array< RankedPlacementScoreStruct > placementScoringData
} file


void function Sh_RankedScoring_Init()
{
	Ranked_InitPlacementScoring()
}

void function Ranked_InitPlacementScoring()
{
	
	var dataTable = GetDataTable( $"datatable/ranked_placement_scoring.rpak" ) 
	int numRows   = GetDataTableRowCount( dataTable )

	file.placementScoringData.clear()








	for ( int i = 0; i < numRows; ++i )
	{
		RankedPlacementScoreStruct placementScoringData
		placementScoringData.placementPosition            = GetDataTableInt( dataTable, i, GetDataTableColumnByName( dataTable, "placement" ) )
		placementScoringData.placementPoints              = GetDataTableInt( dataTable, i, GetDataTableColumnByName( dataTable, "placementPoints" ) )

		placementScoringData.pointsPerKill            = GetDataTableInt( dataTable, i, GetDataTableColumnByName( dataTable, "pointsPerKill" ) )

		file.placementScoringData.append( placementScoringData )











	}
}


bool function Ranked_HasCompletedProvisionalMatches( entity player )
{
	return ( Ranked_GetNumProvisionalMatchesCompleted( player ) >= Ranked_GetNumProvisionalMatchesRequired() )
}

int function Ranked_GetNumProvisionalMatchesRequired()
{
	if ( GetConVarBool( RANKED_PROVISIONAL_MATCH_CONVAR_KILL_SWITCH ) )
		return 0

	return GetCurrentPlaylistVarInt( "ranked_num_provisional_matches", RANKED_NUM_PROVISIONAL_MATCHES )
}

int function Ranked_GetNumProvisionalMatchesCompleted( entity player )
{






	if ( !IsConnected() )
		return 0


	return Ranked_GetXProgMergedPersistenceData( player, RANKED_PROVISIONAL_MATCH_COUNT_PERSISTENCE_VAR_NAME )
}


array < int > function Ranked_GetProvisionalMatchResult ( entity player )
{
	array < int > results






		if ( !IsConnected() )
			return results


	for ( int i = 0 ; i < Ranked_GetNumProvisionalMatchesRequired(); i++ )
	{
		results.append ( Ranked_GetXProgMergedPersistenceData( player, format ( RANKED_PROVISONAL_MATCH_RESULTS_FORMAT_STRINGS, string ( i ) ) ) )
	}

	
	
	
	
	return results

	unreachable
}














































int function Ranked_GetPlayerTop5StreakCount ( entity player )
{






		if ( !IsConnected() )
			return 0


	return Ranked_GetXProgMergedPersistenceData( player, RANKED_TOP_5_STREAK_PERSISTENCE_VAR_NAME )
}


int function Ranked_GetPointsForPlacement( int placement )
{
	int lookupPlacement    = minint( file.placementScoringData.len() - 1, placement )
	int csvValue           = file.placementScoringData[ lookupPlacement ].placementPoints
	string playlistVarName = "rankedPointsForPlacement_" + lookupPlacement

	return GetCurrentPlaylistVarInt( playlistVarName, csvValue )
}


int function RankedGetPointsForKillsByPlacement ( int placement )
{
	int lookupPlacement    = minint( file.placementScoringData.len() - 1, placement )
	int csvValue           = file.placementScoringData[ lookupPlacement ].pointsPerKill
	string playlistVarName = "rankedPointsForKillsByPlacement_" + lookupPlacement

	return GetCurrentPlaylistVarInt( playlistVarName, csvValue )
}









































float function Ranked_GetParticipationMutlipler( )
{
	return GetCurrentPlaylistVarFloat( "ranked_participation_mod", PARTICIPATION_MODIFIER )
}


int function Ranked_GetPenaltyPointsForAbandon( SharedRankedDivisionData currentRank )



{
	
	string playlistVarString      = "ranked_abandon_cost"


		return GetCurrentPlaylistVarInt( playlistVarString, Ranked_GetCostForEntry( currentRank ) )



}


















































































































































































float function GetHighEndLostMultiplier ()
{
	return GetCurrentPlaylistVarFloat( "ranked_tuning_var_high_end_multiplier", HIGH_END_LOST_MULTIPLIER ) 
}


int function Ranked_GetTop5StreakBonusValue ()
{
	return GetCurrentPlaylistVarInt ( "rankedtop5streakbonusvalue", RANKED_BONUS_TOP5STREAK )
}

int function Ranked_GetTop5StreakMax ()
{
	return GetCurrentPlaylistVarInt ( "rankedtop5streakbonusvalue", RANKED_BONUS_TOP5STREAK_MAX )
}

float function Ranked_GetSoftKillCapMod ()
{
	return GetCurrentPlaylistVarFloat ( "rankedSoftKillCapMod", RANKED_BONUS_KILL_SOFT_CAP_MOD )
}

int function Ranked_GetSoftKillCapStartCount ()
{
	return GetCurrentPlaylistVarInt ( "rankedSoftKillCapStartCount", RANKED_BONUS_KILL_SOFT_CAP_COUNT_START )
}

float function Ranked_GetFirstKillBonusMod ()
{
	return GetCurrentPlaylistVarFloat ( "rankedFirstKillBonusMod", RANKED_BONUS_FIRST_KILL_MOD )
}

int function Ranked_GetDemoCrewBonus ()
{
	return GetCurrentPlaylistVarInt ( "rankedDemoCrewBonus", RANKED_BONUS_DEMO_CREW_BONUS )
}

int function Ranked_GetDemoCrewPlacement ()
{
	return GetCurrentPlaylistVarInt ( "rankedDemoCrewPlacement", RANKED_BONUS_DEMO_CREW_PLACEMENT )
}

int function Ranked_GetDemoCrewKills ()
{
	return GetCurrentPlaylistVarInt ( "rankedDemoCrewKills", RANKED_BONUS_DEMO_CREW_KILLS )
}

int function Ranked_GetTop5StreakBonusValueFromStreak ( int streakCount )
{
	int streakSlope  = GetCurrentPlaylistVarInt ( "ranked_top5_StreakBonus", RANKED_BONUS_TOP5STREAK ) 
	int streakCap =  GetCurrentPlaylistVarInt ( "ranked_top5_StreakBonusCap", RANKED_BONUS_TOP5STREAK_MAX ) 
	return maxint ( 0 , ( minint ( streakCount, streakCap)  - 1 ) *  streakSlope )
}

int function Ranked_GetLivingLegendBonus ( int kills, int placement )
{
	if ( kills >= Ranked_GetDemoCrewKills() && placement <= Ranked_GetDemoCrewPlacement () )
	{
		return Ranked_GetDemoCrewBonus ()
	}
	return 0
}






float function Ranked_GetHighSkillKillBonusValue (  )
{
	return GetCurrentPlaylistVarFloat ( "ranked_bonus_value_for_high_skill_kill" , RANKED_BONUS_PER_HIGH_SKILL_KILL_VALUE )
}

int function Ranked_GetValueOfFirstKillBonus ( int placement )
{
	return int ( float ( RankedGetPointsForKillsByPlacement ( placement ))  * Ranked_GetFirstKillBonusMod() )
}

int function Ranked_GetPlayerFirstKillBonus ( int killCount , int placement)
{
	if ( killCount > 0  )
	{
		return int ( float ( Ranked_GetValueOfFirstKillBonus ( placement )) * Ranked_GetFirstKillBonusMod () )
	}
	return 0
}

int function Ranked_GetCombatBonusTotal ( int kill, int assist, int participation, int placement )
{
	float count = kill + assist +  ( participation * Ranked_GetParticipationMutlipler() )

	if ( count >  Ranked_GetSoftKillCapStartCount () )
	{
		count = Ranked_GetSoftKillCapStartCount () + ( ( count -  Ranked_GetSoftKillCapStartCount () ) * Ranked_GetSoftKillCapMod () )
	}

	return int ( count * float ( RankedGetPointsForKillsByPlacement ( placement ) ) )
}


