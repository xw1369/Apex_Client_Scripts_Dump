
global function Sh_RankedScoring_Init
global function Ranked_GetPointsForPlacement
global function Ranked_GetPenaltyPointsForAbandon
global function Ranked_GetParticipationMutlipler
global function Ranked_GetNumProvisionalMatchesCompleted
global function Ranked_GetNumProvisionalMatchesRequired
global function Ranked_HasCompletedProvisionalMatches








global function GetHighEndLostMultiplier

global const RANKED_LP_PROMOTION_BONUS = 250
global const RANKED_LP_DEMOTION_PENALITY = 150
global const float PARTICIPATION_MODIFIER = 0.5
global const int RANKED_NUM_PROVISIONAL_MATCHES = 10
global const float HIGH_END_LOST_MULTIPLIER = 1.5

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


		int trialState = 0 

}

global struct RankedPlacementScoreStruct
{
	
	int   placementPosition
	int   placementPoints



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



		file.placementScoringData.append( placementScoringData )







	}
}


bool function Ranked_HasCompletedProvisionalMatches( entity player )
{
	return ( Ranked_GetNumProvisionalMatchesCompleted( player ) >= Ranked_GetNumProvisionalMatchesRequired() )
}

int function Ranked_GetNumProvisionalMatchesRequired()
{
	if ( GetConVarBool( "ranked_disable_placement_matches" ) )
		return 0

	return GetCurrentPlaylistVarInt( "ranked_num_provisional_matches", RANKED_NUM_PROVISIONAL_MATCHES )
}

int function Ranked_GetNumProvisionalMatchesCompleted( entity player )
{






	if ( !IsConnected() )
		return 0



	return Ranked_GetXProgMergedPersistenceData( player, RANKED_PROVISIONAL_MATCH_COUNT_PERSISTENCE_VAR_NAME )








	unreachable
}

int function Ranked_GetPointsForPlacement( int placement )
{
	int lookupPlacement    = minint( file.placementScoringData.len() - 1, placement )
	int csvValue           = file.placementScoringData[ lookupPlacement ].placementPoints
	string playlistVarName = "rankedPointsForPlacement_" + lookupPlacement

	return GetCurrentPlaylistVarInt( playlistVarName, csvValue )
}



















float function Ranked_GetParticipationMutlipler( )
{
	return GetCurrentPlaylistVarFloat( "ranked_participation_mod", PARTICIPATION_MODIFIER )
}

int function Ranked_GetPenaltyPointsForAbandon( )
{
	
	string playlistVarString      = "ranked_abandon_cost"
	return GetCurrentPlaylistVarInt( playlistVarString, Ranked_GetCostForEntry() )
}


















































































































float function GetHighEndLostMultiplier ()
{
	return GetCurrentPlaylistVarFloat( "ranked_tuning_var_high_end_multiplier", HIGH_END_LOST_MULTIPLIER ) 
}