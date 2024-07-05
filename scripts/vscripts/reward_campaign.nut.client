
global function RewardCampaign_Init
global function RewardCampaign_IsEnabled
global function RewardCampaign_GetRegisteredEvents
global function RewardCampaign_GetActiveEvents
global function RewardCampaign_GetActiveRewardCampaign
global function RewardCampaign_GetCurrentTempUnlockChallenges
global function RewardCampaign_GetMetaChallenge
global function RewardCampaign_GetChallengeCollections
global function RewardCampaign_GetChallengeCollection
global function RewardCampaign_GetCollectionMetaChallenges
global function RewardCampaign_GetMainChallenge
global function RewardCampaign_GetStandaloneChallenges
global function RewardCampaign_GetChildChallenges
global function RewardCampaign_GetProgress
global function RewardCampaign_GetLobbyChallengeHeaderBackgroundImage
global function RewardCampaign_GetChallegeGroupName
global function RewardCampaign_GetUpcomingLegendTooltipTitle
global function RewardCampaign_GetUpcomingLegendTooltipDesc
global function RewardCampaign_GetCompletedLegendTooltipTitle
global function RewardCampaign_GetCompletedLegendTooltipDesc
global function RewardCampaign_GetUnlockedLegendTooltipTitle
global function RewardCampaign_GetUnlockedLegendTooltipDesc
global function RewardCampaign_GetTutorialData
global function RewardCampaign_GetChallengeOrderMap

global struct RewardCampaignTutorialData
{
	asset icon
	string title
	string desc
}

const PLAYLIST_KILLSWITCH_VAR = "reward_campaigns_enabled"

struct FileStruct_RewardCampaign
{
	array< ItemFlavor > registeredRewardCampaigns
	array< RewardCampaignTutorialData > rewardCampaignTutorialData
}
FileStruct_RewardCampaign& file





void function RewardCampaign_Init()
{

	FileStruct_RewardCampaign fileStruct
	file = fileStruct

	AddCallback_OnItemFlavorRegistered( eItemType.calevent_reward_campaign, RegisterRewardCampaign )

	AddCallback_OnItemFlavorRegistered( eItemType.calevent_reward_campaign, void function( ItemFlavor event ) {
		foreach ( int index, var tutorialBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), "tutorialBreakdownItems" ) )
		{
			RewardCampaignTutorialData tutorial
			tutorial.icon  = GetSettingsBlockAsset( tutorialBlock, "tutorialIcon" )
			tutorial.title = GetSettingsBlockString( tutorialBlock, "tutorialTitleLocString" )
			tutorial.desc  = GetSettingsBlockString( tutorialBlock, "tutorialBodyTextLocString" )
			file.rewardCampaignTutorialData.append( tutorial )
		}
	})
}

void function RegisterRewardCampaign( ItemFlavor rewardCampaignFlav )
{
	
	ItemFlavor ornull metaChallenge = RegisterItemFlavorFromSettingsAsset( GetGlobalSettingsAsset( ItemFlavor_GetAsset( rewardCampaignFlav ), "progressMetaChallenge" ) )

	
	array< ItemFlavor > challengeCollections = RegisterReferencedItemFlavorsFromArray( rewardCampaignFlav, "challengeCollections", "challengeCollection" )

	
	array< ItemFlavor > challenges = RegisterReferencedItemFlavorsFromArray( rewardCampaignFlav, "challenges", "challengeFlav" )

	file.registeredRewardCampaigns.push( rewardCampaignFlav )
}





bool function RewardCampaign_IsEnabled()
{
	bool isEnabled = GetCurrentPlaylistVarBool( PLAYLIST_KILLSWITCH_VAR, true )
	return isEnabled
}

array< ItemFlavor > function RewardCampaign_GetRegisteredEvents()
{
	return file.registeredRewardCampaigns
}

array< ItemFlavor > function RewardCampaign_GetActiveEvents( int unixTime )
{
	array< ItemFlavor > events
	if ( !RewardCampaign_IsEnabled() )
	{
		return events
	}

	
	Assert( IsItemFlavorRegistrationFinished() )
	foreach ( ItemFlavor ev in file.registeredRewardCampaigns )
	{
		if ( CalEvent_IsActive( ev, unixTime ) )
		{
			events.append( ev )
		}
	}

	
	Assert( events.len() <= 1, "Only 1 concurrently active reward campaign currently supported!" )
	return events
}

ItemFlavor ornull function RewardCampaign_GetActiveRewardCampaign()
{
	array< ItemFlavor > events = RewardCampaign_GetActiveEvents( GetUnixTimestamp() )
	return events.len() > 0 ? events[0] : null
}

array<ItemFlavor> function RewardCampaign_GetCurrentTempUnlockChallenges( entity player )
{
	array<ItemFlavor> challenges = []
	ItemFlavor ornull campaignEvent = RewardCampaign_GetActiveRewardCampaign()
	ItemFlavor ornull tempUnlockEvent = TempUnlock_GetLatestActiveEvent( GetUnixTimestamp() )
	if ( campaignEvent != null && tempUnlockEvent != null )
	{
		expect ItemFlavor( campaignEvent )
		expect ItemFlavor( tempUnlockEvent )

		array<ItemFlavor> collectionFlavs = RewardCampaign_GetChallengeCollections( campaignEvent )
		if ( collectionFlavs.len() < 1 )
		{
			return challenges
		}

		ChallengeCollection collection = ChallengeCollection_GetByGUID( collectionFlavs[0].guid )

		foreach( ChallengeSet set in collection.challengeSets )
		{
			if ( CalEvent_GetStartUnixTime( set.itemFlav ) == CalEvent_GetStartUnixTime( tempUnlockEvent ) )
			{
				array<ItemFlavor> assignedChallenges = GetAssignedChallengesByTimeSpan( player, eChallengeTimeSpanKind.REWARD_CAMPAIGN )
				foreach( ItemFlavor challenge in set.challenges )
				{
					foreach ( assignedChallenge in assignedChallenges )
					{
						
						if ( assignedChallenge.guid == challenge.guid )
						{
							challenges.append( assignedChallenge )
						}
					}
				}
				return challenges
			}
		}
	}
	return challenges
}


ItemFlavor function RewardCampaign_GetMetaChallenge( ItemFlavor rewardCampaignFlav )
{
	Assert( ItemFlavor_GetType( rewardCampaignFlav ) == eItemType.calevent_reward_campaign )
	asset metaChallengeAsset = GetGlobalSettingsAsset( ItemFlavor_GetAsset( rewardCampaignFlav ), "progressMetaChallenge" )
	return GetItemFlavorByAsset( metaChallengeAsset )
}

array<ItemFlavor> function RewardCampaign_GetChallengeCollections( ItemFlavor rewardCampaignFlav )
{
	Assert( ItemFlavor_GetType( rewardCampaignFlav ) == eItemType.calevent_reward_campaign )
	array<ItemFlavor> challengeCollections

	foreach ( var collectionsBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( rewardCampaignFlav ), "challengeCollections" ) )
	{
		asset collectionAsset = GetSettingsBlockAsset( collectionsBlock, "challengeCollection" )
		if ( IsValidItemFlavorSettingsAsset( collectionAsset ) )
		{
			ItemFlavor collectionFlav = GetItemFlavorByAsset( collectionAsset )
			challengeCollections.append( collectionFlav )
		}
	}

	return challengeCollections
}

ItemFlavor ornull function RewardCampaign_GetChallengeCollection( ItemFlavor rewardCampaignFlav )
{
	Assert( ItemFlavor_GetType( rewardCampaignFlav ) == eItemType.calevent_reward_campaign )
	array<ItemFlavor> challengeCollectionFlavs = RewardCampaign_GetChallengeCollections( rewardCampaignFlav )

	if ( challengeCollectionFlavs.len() > 0 )
	{
		return challengeCollectionFlavs[0]
	}
	return null
}


array<ItemFlavor> function RewardCampaign_GetCollectionMetaChallenges( ItemFlavor rewardCampaignFlav )
{
	Assert( ItemFlavor_GetType( rewardCampaignFlav ) == eItemType.calevent_reward_campaign )
	array<ItemFlavor> challenges

	foreach ( var collectionsBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( rewardCampaignFlav ), "challengeCollections" ) )
	{
		asset collectionAsset = GetSettingsBlockAsset( collectionsBlock, "challengeCollection" )
		if ( IsValidItemFlavorSettingsAsset( collectionAsset ) )
		{
			challenges.append( GetItemFlavorByAsset( GetGlobalSettingsAsset( collectionAsset, "mainChallengeFlav" ) ) )
		}
	}
	return challenges
}


ItemFlavor function RewardCampaign_GetMainChallenge( entity player, ItemFlavor rewardCampaignFlav )
{
	Assert( ItemFlavor_GetType( rewardCampaignFlav ) == eItemType.calevent_reward_campaign )
	foreach ( var collectionsBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( rewardCampaignFlav ), "challengeCollections" ) )
	{
		asset collectionAsset = GetSettingsBlockAsset( collectionsBlock, "challengeCollection" )
		if ( IsValidItemFlavorSettingsAsset( collectionAsset ) )
			return GetItemFlavorByAsset( GetGlobalSettingsAsset( collectionAsset, "mainChallengeFlav" ) )
	}

	unreachable
}


array<ItemFlavor> function RewardCampaign_GetStandaloneChallenges( ItemFlavor rewardCampaignFlav )
{
	Assert( ItemFlavor_GetType( rewardCampaignFlav ) == eItemType.calevent_reward_campaign )
	array<ItemFlavor> challenges
	foreach ( var challengeBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( rewardCampaignFlav ), "challenges" ) )
	{
		asset collectionAsset = GetSettingsBlockAsset( challengeBlock, "challengeFlav" )
		if ( IsValidItemFlavorSettingsAsset( collectionAsset ) )
		{
			challenges.append( GetItemFlavorByAsset( collectionAsset ) )
		}
	}
	return challenges
}


array<ItemFlavor> function RewardCampaign_GetChildChallenges( ItemFlavor rewardCampaignFlav )
{
	Assert( ItemFlavor_GetType( rewardCampaignFlav ) == eItemType.calevent_reward_campaign )
	array<ItemFlavor> challenges

	challenges.extend( RewardCampaign_GetCollectionMetaChallenges( rewardCampaignFlav ) )
	challenges.extend( RewardCampaign_GetStandaloneChallenges( rewardCampaignFlav ) )

	return challenges
}

int function RewardCampaign_GetProgress( entity player, ItemFlavor rewardCampaignFlav )
{
	Assert( ItemFlavor_GetType( rewardCampaignFlav ) == eItemType.calevent_reward_campaign )
	int numCompleted = 0

	foreach ( ItemFlavor challenge in RewardCampaign_GetChildChallenges( rewardCampaignFlav ) )
	{
		if ( Challenge_IsAssigned( player, challenge) && Challenge_IsComplete( player, challenge ) )
		{
			++numCompleted
		}
	}

	return numCompleted
}

asset function RewardCampaign_GetLobbyChallengeHeaderBackgroundImage( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_reward_campaign )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "headerImageBackground" )
}

string function RewardCampaign_GetChallegeGroupName( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_reward_campaign )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "challengeGroupName" )
}

string function RewardCampaign_GetUpcomingLegendTooltipTitle( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_reward_campaign )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "tempUpcomingCampaignTooltipTitle" )
}

string function RewardCampaign_GetUpcomingLegendTooltipDesc( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_reward_campaign )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "tempUpcomingCampaignTooltipDesc" )
}

string function RewardCampaign_GetCompletedLegendTooltipTitle( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_reward_campaign )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "tempCompletedCampaignTooltipTitle" )
}

string function RewardCampaign_GetCompletedLegendTooltipDesc( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_reward_campaign )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "tempCompletedCampaignTooltipDesc" )
}

string function RewardCampaign_GetUnlockedLegendTooltipTitle( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_reward_campaign )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "tempUnlockCampaignTooltipTitle" )
}

string function RewardCampaign_GetUnlockedLegendTooltipDesc( ItemFlavor event, bool isOwned )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_reward_campaign )
	return isOwned ? GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "tempUnlockCampaignTooltipDescOwned" ) : GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "tempUnlockCampaignTooltipDesc" )
}

array<RewardCampaignTutorialData> function RewardCampaign_GetTutorialData( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_reward_campaign )
	return file.rewardCampaignTutorialData
}


table<SettingsAssetGUID, int> function RewardCampaign_GetChallengeOrderMap( ItemFlavor rc )
{
	Assert( ItemFlavor_GetType( rc ) == eItemType.calevent_reward_campaign )
	array< ItemFlavor > challenges

	
    
	challenges.append( RewardCampaign_GetMetaChallenge( rc ) )

	
	challenges.extend( RewardCampaign_GetStandaloneChallenges( rc ) )

	
	foreach ( var collectionsBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( rc ), "challengeCollections" ) )
	{
		asset collectionAsset = GetSettingsBlockAsset( collectionsBlock, "challengeCollection" )
		if ( IsValidItemFlavorSettingsAsset( collectionAsset ) )
		{
			ItemFlavor challengeCollectionFlav = GetItemFlavorByAsset( collectionAsset )

			
			ChallengeCollection challengeCollection = ChallengeCollection_GetByGUID( challengeCollectionFlav.guid )
			challenges.append( challengeCollection.mainChallenge )

			
			foreach ( ChallengeSet challengeSet in challengeCollection.challengeSets )
			{
				
				challenges.append( challengeSet.mainChallenge )
				
				challenges.extend( challengeSet.challenges )
			}
		}
	}

	table< SettingsAssetGUID, int > guidToIndexMap
	foreach ( int index, ItemFlavor challengeFlav in challenges )
	{
		guidToIndexMap[ challengeFlav.guid ] <- index
	}
	return guidToIndexMap
}