


















global function CLUI_Ranked_Init
global function PopulateRuiWithRankedBadgeDetails
global function PopulateRuiWithPreviousSeasonRankedBadgeDetails
global function CreateNestedRankedRui
global function SharedRanked_FillInRuiEmblemText



global function ShRanked_RegisterNetworkFunctions
global function ServerToClient_Ranked_UpdatePlayerPrevRankedScore
global function ServerToClient_Ranked_UpdatePlayerPrevLadderPos


struct
{





} file

void function CLUI_Ranked_Init()
{

		if ( !IsRankedGame() )
			return

		AddCallback_OnScoreboardCreated( OnScoreboardCreated )
		AddCallback_OnGameStateChanged( OnGameStateChanged )

		Obituary_SetHorizontalOffset( -25 ) 
		AddOnSpectatorTargetChangedCallback( Ranked_OnSpectateTargetChanged )

}


void function ShRanked_RegisterNetworkFunctions()
{
	if ( !IsRankedGame() )
		return

	RegisterNetVarIntChangeCallback( "nv_currentRankedScore", OnRankedScoreChanged )
	RegisterNetVarIntChangeCallback( "nv_currentRankedLadderPosition", OnRankedLadderPositionChanged )
}

void function ServerToClient_Ranked_UpdatePlayerPrevRankedScore ( entity player, int score )
{
	if ( IsLobby() )
		return

	if ( score == SHARED_RANKED_INVALID_RANK_SCORE )
		return

	if ( !IsValid( player ) )
		return

	EHI playerEHI = ToEHI( player )
	Ranked_UpdateEHIRankScorePrevSeason( playerEHI, score )
}

void function ServerToClient_Ranked_UpdatePlayerPrevLadderPos ( entity player, int pos )
{
	if ( IsLobby() )
		return

	if ( pos == SHARED_RANKED_INVALID_LADDER_POSITION )
		return

	if ( !IsValid( player ) )
		return

	EHI playerEHI = ToEHI( player )
	Ranked_UpdateEHIRankedLadderPositionPrevSeason( playerEHI, pos )
}

void function OnRankedScoreChanged( entity player, int new )
{

	if ( IsLobby() )
		return

	if ( new == SHARED_RANKED_INVALID_RANK_SCORE )
		return

	EHI playerEHI = ToEHI( player )
	Ranked_UpdateEHIRankScore( playerEHI, new )
	RunUIScript( "Ranked_UpdateEHIRankScore", playerEHI, new )

	if ( player != GetLocalViewPlayer() )
		return

	SetRankedIcon( new, Ranked_GetLadderPosition( player ) )
}



void function OnScoreboardCreated()
{
	if ( GetLocalViewPlayer() == null )
		return

	int score     = GetPlayerRankScore( GetLocalViewPlayer() )
	int ladderPos = Ranked_GetLadderPosition( GetLocalViewPlayer() )
	SetRankedIcon( score, ladderPos )
}




void function OnGameStateChanged( int newVal )
{
	if ( IsLobby() )
		return

	Assert( IsRankedGame() )

	var rui       = ClGameState_GetRui()
	int gameState = newVal
	if ( gameState >= eGameState.Prematch )
	{
		RuiSetBool( rui, "showRanked", true )
		OnScoreboardCreated()
	}
}




void function OnRankedLadderPositionChanged( entity player, int new )
{
	if ( IsLobby() )
		return

	if ( new == SHARED_RANKED_INVALID_LADDER_POSITION ) 
		return

	EHI playerEHI = ToEHI( player )
	Ranked_UpdateEHIRankedLadderPosition( playerEHI, new )
	RunUIScript( "Ranked_UpdateEHIRankedLadderPosition", playerEHI, new )

	if ( player != GetLocalViewPlayer() )
		return

	SetRankedIcon( GetPlayerRankScore( player ), new )
}




void function SetRankedIcon( int score, int ladderPos )
{
	var rui = ClGameState_GetRui()
	if ( rui == null )
		return

	if ( score < 0 )
		return

	SharedRankedDivisionData data = GetCurrentRankedDivisionFromScoreAndLadderPosition( score, ladderPos )
	PopulateRuiWithRankedBadgeDetails( rui, score, ladderPos )

	if ( GetLocalViewPlayer() != null )
		RuiTrackInt( rui, "inMatchRankScoreProgress", GetLocalViewPlayer(), RUI_TRACK_SCRIPT_NETWORK_VAR_INT, GetNetworkedVariableIndex( "inMatchRankScoreProgress" ) )
}




void function Ranked_OnSpectateTargetChanged( entity spectatingPlayer, entity prevSpectatorTarget, entity newSpectatorTarget )
{
	
	if ( IsValid( newSpectatorTarget ) && newSpectatorTarget.IsPlayer() )
		SetRankedIcon( GetPlayerRankScore( newSpectatorTarget ), Ranked_GetLadderPosition( newSpectatorTarget ) )
}
































































































































































































































































































































































































































































































































































void function PopulateRuiWithRankedProvisionalBadgeDetails( var rui, int numMatchesCompleted, int rankScore, int ladderPosition, bool isNested = false, int maxPips = 10, bool useDynamicPips = false, bool isPromotional = false )
{
	
	RuiSetInt( rui, "startPip", 0 )
	RuiSetInt( rui, "maxPips", maxPips )
	RuiSetBool( rui, "useDynamicPips", useDynamicPips )
	RuiSetBool( rui, "isPromotional", isPromotional )

	RuiSetInt( rui, "placementProgress", numMatchesCompleted )
	for ( int i = 0; i < numMatchesCompleted; i++ )
		RuiSetBool( rui, format("wonGame%d", i ) , true )

	SharedRankedDivisionData playerRank = GetCurrentRankedDivisionFromScoreAndLadderPosition( rankScore, ladderPosition )
	RuiSetImage( rui, "rankedIcon", playerRank.tier.icon )
	RuiSetInt( rui, "emblemDisplayMode", playerRank.emblemDisplayMode )

	switch( playerRank.emblemDisplayMode )
	{
		case emblemDisplayMode.DISPLAY_DIVISION:
		{
			RuiSetString( rui, "emblemText", playerRank.emblemText )
			break
		}

		case emblemDisplayMode.DISPLAY_RP:
		{
			string rankScoreShortened = FormatAndLocalizeNumber( "1", float( rankScore ), IsTenThousandOrMore( rankScore ) )
			RuiSetString( rui, "emblemText", Localize( "#RANKED_POINTS_GENERIC", rankScoreShortened ) )
			break
		}

		case emblemDisplayMode.DISPLAY_LADDER_POSITION:
		{
			string ladderPosShortened
			if ( ladderPosition == SHARED_RANKED_INVALID_LADDER_POSITION )
				ladderPosShortened = ""
			else
				ladderPosShortened = Localize( "#RANKED_LADDER_POSITION_DISPLAY", FormatAndLocalizeNumber( "1", float( ladderPosition ), IsTenThousandOrMore( ladderPosition ) ) )

			RuiSetString( rui, "emblemText", ladderPosShortened )
			break
		}

		case emblemDisplayMode.NONE:
		default:
		{
			RuiSetString( rui, "emblemText", "" )
			break
		}
	}

	if ( !isNested )
	{
		SharedRankedDivisionData currentRank = GetCurrentRankedDivisionFromScoreAndLadderPosition( SHARED_RANKED_ROOKIE_FLOOR_VALUE, SHARED_RANKED_INVALID_LADDER_POSITION )
		RuiDestroyNestedIfAlive( rui, "rankedBadgeHandle" )
		CreateNestedRankedRui( rui, currentRank.tier, "rankedBadgeHandle", SHARED_RANKED_ROOKIE_FLOOR_VALUE, SHARED_RANKED_INVALID_LADDER_POSITION )
	}
}

void function PopulateRuiWithRankedBadgeDetails( var rui, int rankScore, int ladderPosition, bool isNested = false )
{
	SharedRankedDivisionData currentRank = GetCurrentRankedDivisionFromScoreAndLadderPosition( rankScore, ladderPosition )
	
	SharedRankedTierData currentTier     = currentRank.tier
	RuiSetImage( rui, "rankedIcon", currentTier.icon )
	if ( currentTier.isLadderOnlyTier ) 
	{
		SharedRankedTierData tierByScore = GetCurrentRankedDivisionFromScore( rankScore ).tier
		RuiSetInt( rui, "rankedIconState", tierByScore.index + 1 )
	}
	else
	{
		RuiSetInt( rui, "rankedIconState", currentTier.index )
	}

	SharedRanked_FillInRuiEmblemText( rui, currentRank, rankScore, ladderPosition )

	if ( !isNested )
	{
		RuiDestroyNestedIfAlive( rui, "rankedBadgeHandle" )
		CreateNestedRankedRui( rui, currentRank.tier, "rankedBadgeHandle", rankScore, ladderPosition )
	}
}

void function PopulateRuiWithPreviousSeasonRankedBadgeDetails( var rui, int rankScore, int ladderPosition, bool isNested = false )
{
	SharedRankedDivisionData currentRank = GetCurrentRankedDivisionFromScoreAndLadderPosition( rankScore, ladderPosition )
	
	SharedRankedTierData currentTier     = currentRank.tier
	RuiSetImage( rui, "rankedIconPrev", currentTier.icon )
	if ( currentTier.isLadderOnlyTier ) 
	{
		SharedRankedTierData tierByScore = GetCurrentRankedDivisionFromScore( rankScore ).tier
		RuiSetInt( rui, "rankedIconStatePrev", tierByScore.index + 1 )
	}
	else
	{
		RuiSetInt( rui, "rankedIconStatePrev", currentTier.index )
	}

	SharedRanked_FillInRuiEmblemText( rui, currentRank, rankScore, ladderPosition, "Prev" )

	if ( !isNested )
	{
		RuiDestroyNestedIfAlive( rui, "rankedBadgeHandlePrev" )
		CreateNestedRankedRui( rui, currentRank.tier, "rankedBadgeHandlePrev", rankScore, ladderPosition )
	}
}


void function PopulateRuiWithHistoricalRankedBadgeDetails( var rui, int rankScore, int ladderPosition, string rankedSeasonGUID, bool isNested = false )
{
	SharedRankedDivisionData historicalRank = Ranked_GetHistoricalRankedDivisionFromScoreAndLadderPosition( rankScore, ladderPosition, rankedSeasonGUID )
	SharedRankedTierData historicalTier     = historicalRank.tier
	RuiSetImage( rui, "rankedIcon", historicalTier.icon )
	

	if ( historicalTier.isLadderOnlyTier ) 
	{
		SharedRankedTierData tierByScore = Ranked_GetHistoricalRankedDivisionFromScore( rankScore, rankedSeasonGUID ).tier
		RuiSetInt( rui, "rankedIconState", tierByScore.index + 1 )
	}
	else
	{
		RuiSetInt( rui, "rankedIconState", historicalTier.index )
	}

	SharedRanked_FillInRuiEmblemText( rui, historicalRank, rankScore, ladderPosition )

	if ( !isNested )
	{
		RuiDestroyNestedIfAlive( rui, "rankedBadgeHandle" )
		CreateNestedHistoricalRankedRui( rui, historicalRank.tier, rankedSeasonGUID, "rankedBadgeHandle", rankScore, ladderPosition )
	}
}


var function CreateNestedRankedRui( var pRui, SharedRankedTierData tier, string varName, int score, int ladderPosition )
{
	
	entity uiPlayer = GetLocalClientPlayer()
	if ( !Ranked_HasCompletedProvisionalMatches( uiPlayer ) )
	{
		var rui = RuiCreateNested( pRui, varName, RANKED_PLACEMENT_BADGE )
		int numMatchesCompleted = Ranked_GetNumProvisionalMatchesCompleted( uiPlayer )
		PopulateRuiWithRankedProvisionalBadgeDetails( rui, numMatchesCompleted, score, ladderPosition, true )
		return rui
	}

	else if ( RankedTrials_PlayerHasIncompleteTrial( uiPlayer ) )
	{
		var rui = RuiCreateNested( pRui, varName, RANKED_PLACEMENT_BADGE )

		ItemFlavor currentTrial = RankedTrials_GetAssignedTrial( uiPlayer )
		int numMatchesCompleted = RankedTrials_GetGamesPlayedInTrialsState( uiPlayer )
		int maxMatches = RankedTrials_GetGamesAllowedInTrialsState( uiPlayer, currentTrial )

		PopulateRuiWithRankedProvisionalBadgeDetails( rui, numMatchesCompleted, score, ladderPosition, true, maxMatches, true, true )
		return rui
	}

	else
	{
		var rui = RuiCreateNested( pRui, varName, tier.iconRuiAsset )
		PopulateRuiWithRankedBadgeDetails( rui, score, ladderPosition, true )
		return rui
	}

	unreachable
}


var function CreateNestedHistoricalRankedRui( var pRui, SharedRankedTierData tier, string rankedSeasonGUID, string varName, int score, int ladderPosition )
{
	var rui = RuiCreateNested( pRui, varName, tier.iconRuiAsset )

	PopulateRuiWithHistoricalRankedBadgeDetails( rui, score, ladderPosition, rankedSeasonGUID, true )

	return rui
}


void function SharedRanked_FillInRuiEmblemText( var rui, SharedRankedDivisionData divData, int rankScore, int ladderPosition, string ruiArgumentPostFix = "" )
{
	
	entity uiPlayer = GetLocalClientPlayer()
	bool useProvisionalFix = !Ranked_HasCompletedProvisionalMatches( uiPlayer )

		useProvisionalFix = !Ranked_HasCompletedProvisionalMatches( uiPlayer ) || RankedTrials_PlayerHasIncompleteTrial( uiPlayer )


	if ( useProvisionalFix )
	{
		RuiSetInt( rui, "emblemDisplayMode" + ruiArgumentPostFix, emblemDisplayMode.NONE )
		RuiSetString( rui, "emblemText" + ruiArgumentPostFix, "" )
		return
	}

	RuiSetInt( rui, "emblemDisplayMode" + ruiArgumentPostFix, divData.emblemDisplayMode )
	switch( divData.emblemDisplayMode )
	{
		case emblemDisplayMode.DISPLAY_DIVISION:
		{
			RuiSetString( rui, "emblemText" + ruiArgumentPostFix, divData.emblemText )
			break
		}

		case emblemDisplayMode.DISPLAY_RP:
		{
			string rankScoreShortened = FormatAndLocalizeNumber( "1", float( rankScore ), IsTenThousandOrMore( rankScore ) )
			RuiSetString( rui, "emblemText" + ruiArgumentPostFix, Localize( "#RANKED_POINTS_GENERIC", rankScoreShortened ) )
			break
		}

		case emblemDisplayMode.DISPLAY_LADDER_POSITION:
		{
			string ladderPosShortened
			if ( ladderPosition == SHARED_RANKED_INVALID_LADDER_POSITION )
				ladderPosShortened = ""
			else
				ladderPosShortened = Localize( "#RANKED_LADDER_POSITION_DISPLAY", FormatAndLocalizeNumber( "1", float( ladderPosition ), IsTenThousandOrMore( ladderPosition ) ) )

			RuiSetString( rui, "emblemText" + ruiArgumentPostFix, ladderPosShortened )
			break
		}

		case emblemDisplayMode.NONE:
		default:
		{
			RuiSetString( rui, "emblemText" + ruiArgumentPostFix, "" )
			break
		}
	}
}
































































