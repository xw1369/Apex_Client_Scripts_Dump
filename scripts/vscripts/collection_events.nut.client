global function CollectionEvents_Init
global function GetActiveCollectionEvent
global function CollectionEvent_GetChallenges 
global function CollectionEvent_GetFrontPageRewardBoxTitle
global function CollectionEvent_GetCollectionName
global function CollectionEvent_GetMainPackFlav
global function CollectionEvent_GetMainPackShortPluralName
global function CollectionEvent_GetMainPackImage
global function CollectionEvent_GetFrontPageGRXOfferLocation
global function CollectionEvent_GetRewardGroups
global function CollectionEvent_IsGivenItemFlavorReward
global function CollectionEvent_GetAboutText 
global function CollectionEvent_GetStoreEventSectionMainImage
global function CollectionEvent_GetStoreEventSectionMainText
global function CollectionEvent_GetStoreEventSectionSubText
global function CollectionEvent_GetMainIcon
global function CollectionEvent_GetMainThemeCol
global function CollectionEvent_GetFrontPageBGTintCol
global function CollectionEvent_GetFrontPageTitleCol
global function CollectionEvent_GetFrontPageSubtitleCol
global function CollectionEvent_GetFrontPageTimeRemainingCol
global function CollectionEvent_GetBGPatternImage
global function CollectionEvent_GetBGTabPatternImage
global function CollectionEvent_GetTabLeftSideImage 
global function CollectionEvent_GetTabCenterImage 
global function CollectionEvent_GetTabRightSideImage 
global function CollectionEvent_GetTabImageSelectedAlpha
global function CollectionEvent_GetTabImageUnselectedAlpha
global function CollectionEvent_GetTabCenterRui 
global function CollectionEvent_GetTabBGDefaultCol 
global function CollectionEvent_GetTabBarDefaultCol 
global function CollectionEvent_GetTabBGFocusedCol 
global function CollectionEvent_GetTabTextDefaultCol 
global function CollectionEvent_GetTabBarFocusedCol 
global function CollectionEvent_GetTabGlowFocusedCol 
global function CollectionEvent_GetTabBGSelectedCol 
global function CollectionEvent_GetTabBarSelectedCol 
global function CollectionEvent_GetTabTextSelectedCol 
global function CollectionEvent_GetAboutPageSpecialTextCol 
global function CollectionEvent_GetHeaderIcon 


global function HeirloomEvent_GetItemCount
global function HeirloomEvent_GetCurrentRemainingItemCount
global function HeirloomEvent_GetPrimaryCompletionRewardItem
global function HeirloomEvent_GetCompletionRewardPack
global function HeirloomEvent_GetCompletionSequenceName
global function HeirloomEvent_AwardHeirloomShards
global function HeirloomEvent_IsRewardMythicSkin





















global function CollectionEvent_GetFrontTabText



















global struct CollectionEventRewardGroup
{
	string ref
	int    quality = -1
	
	

	array<ItemFlavor> rewards
}


	global const array< int > HEIRLOOM_EVENTS = [ eItemType.calevent_collection, eItemType.calevent_themedshop, eItemType.calevent_milestone ]










struct FileStruct_LifetimeLevel
{





	table<ItemFlavor, array<ItemFlavor> > eventChallengesMap
}


FileStruct_LifetimeLevel fileLevel 

















void function CollectionEvents_Init()
{





	AddCallback_OnItemFlavorRegistered( eItemType.calevent_collection, void function( ItemFlavor ev ) {
		fileLevel.eventChallengesMap[ev] <- RegisterReferencedItemFlavorsFromArray( ev, "challenges", "flavor" )
		foreach ( int challengeSortOrdinal, ItemFlavor challengeFlav in fileLevel.eventChallengesMap[ev] )
			RegisterChallengeSource( challengeFlav, ev, challengeSortOrdinal )
	} )





}









ItemFlavor ornull function GetActiveCollectionEvent( int t )
{
	Assert( IsItemFlavorRegistrationFinished() )
	ItemFlavor ornull event = null
	foreach ( ItemFlavor ev in GetAllItemFlavorsOfType( eItemType.calevent_collection ) )
	{
		if ( !CalEvent_IsActive( ev, t ) )
			continue

		Assert( event == null, format( "Multiple collection events are active!! (%s, %s)", string(ItemFlavor_GetAsset( expect ItemFlavor(event) )), string(ItemFlavor_GetAsset( ev )) ) )
		event = ev
	}
	return event
}




array<ItemFlavor> function CollectionEvent_GetLoginRewards( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )

	array<ItemFlavor> rewards = []
	foreach ( var rewardBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), "loginRewards" ) )
	{
		asset rewardAsset = GetSettingsBlockAsset( rewardBlock, "flavor" )
		if ( IsValidItemFlavorSettingsAsset( rewardAsset ) )
			rewards.append( GetItemFlavorByAsset( rewardAsset ) )
	}
	return rewards
}




array<ItemFlavor> function CollectionEvent_GetChallenges( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )

	return fileLevel.eventChallengesMap[event]
}




string function CollectionEvent_GetFrontPageRewardBoxTitle( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "frontPageRewardBoxTitle" )
}























string function CollectionEvent_GetCollectionName( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "collectionName" )
}



ItemFlavor function CollectionEvent_GetMainPackFlav( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetItemFlavorByAsset( GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "mainPackFlav" ) )
}




string function CollectionEvent_GetMainPackShortPluralName( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "mainPackShortPluralName" )
}




asset function CollectionEvent_GetMainPackImage( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "mainPackImage" )
}




bool function HeirloomEvent_AwardHeirloomShards( ItemFlavor event )
{
	Assert( HEIRLOOM_EVENTS.contains( ItemFlavor_GetType( event ) ) )
	return GetGlobalSettingsBool( ItemFlavor_GetAsset( event ), "awardHeirloomShards" )
}




ItemFlavor function HeirloomEvent_GetPrimaryCompletionRewardItem( ItemFlavor event )
{
	Assert( HEIRLOOM_EVENTS.contains( ItemFlavor_GetType( event ) ) )

	if ( HeirloomEvent_AwardHeirloomShards( event ) )
		return GetItemFlavorByAsset( $"settings/itemflav/currency_bundle/heirloom.rpak" )

	return GetItemFlavorByAsset( GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "primaryCompletionRewardItem" ) )
}



bool function HeirloomEvent_IsRewardMythicSkin( ItemFlavor event )
{
	ItemFlavor primaryRewardItem =  HeirloomEvent_GetPrimaryCompletionRewardItem( event )
	return Mythics_IsItemFlavorMythicSkin( primaryRewardItem )
}




ItemFlavor function HeirloomEvent_GetCompletionRewardPack( ItemFlavor event )
{
	Assert( HEIRLOOM_EVENTS.contains( ItemFlavor_GetType( event ) ) )

	if ( HeirloomEvent_AwardHeirloomShards( event ) )
		return GetItemFlavorByAsset( $"settings/itemflav/pack/heirloom_shards.rpak" )

	return GetItemFlavorByAsset( GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "completionRewardPack" ) )
}




string function HeirloomEvent_GetCompletionSequenceName( ItemFlavor event )
{
	Assert( HEIRLOOM_EVENTS.contains( ItemFlavor_GetType( event ) ) )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "completionSequenceName" )
}




string function CollectionEvent_GetFrontPageGRXOfferLocation( ItemFlavor event, bool isRestricted = false )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "frontGRXOfferLocation" ) 
}



array<CollectionEventRewardGroup> function CollectionEvent_GetRewardGroups( ItemFlavor event )
{

		
		Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection || ItemFlavor_GetType( event ) == eItemType.calevent_milestone )



	array<CollectionEventRewardGroup> groups = []
	foreach ( var groupBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), "rewardGroups" ) )
	{
		CollectionEventRewardGroup group
		group.ref = GetSettingsBlockString( groupBlock, "ref" )
		group.quality = eRarityTier[GetSettingsBlockString( groupBlock, "quality" )]
		
		
		foreach ( var rewardBlock in IterateSettingsArray( GetSettingsBlockArray( groupBlock, "rewards" ) ) )
			group.rewards.append( GetItemFlavorByAsset( GetSettingsBlockAsset( rewardBlock, "flavor" ) ) )

		groups.append( group )
	}
	return groups
}



bool function CollectionEvent_IsGivenItemFlavorReward( ItemFlavor event, ItemFlavor item )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )

	array<CollectionEventRewardGroup> rewardGroups = CollectionEvent_GetRewardGroups( event )

	foreach( CollectionEventRewardGroup group in rewardGroups )
	{
		foreach( ItemFlavor flav in group.rewards )
		{
			if ( flav.guid == item.guid )
			{
				return true
			}
		}
	}

	return false
}




array<string> function CollectionEvent_GetAboutText( ItemFlavor event, bool restricted )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )

	array<string> aboutText = []
	string key              = (restricted ? "aboutTextRestricted" : "aboutTextStandard")
	foreach ( var aboutBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), key ) )
		aboutText.append( GetSettingsBlockString( aboutBlock, "text" ) )
	return aboutText
}



asset function CollectionEvent_GetStoreEventSectionMainImage( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "storeEventSectionMainImage" )
}



string function CollectionEvent_GetStoreEventSectionMainText( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "storeEventSectionMainText" )
}



string function CollectionEvent_GetStoreEventSectionSubText( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "storeEventSectionSubText" )
}



void function CollectionEvent_GetMainIcon( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
}




vector function CollectionEvent_GetMainThemeCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "mainThemeCol" )
}




vector function CollectionEvent_GetFrontPageBGTintCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "frontPageBGTintCol" )
}




vector function CollectionEvent_GetFrontPageTitleCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "frontPageTitleCol" )
}




vector function CollectionEvent_GetFrontPageSubtitleCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "frontPageSubtitleCol" )
}




vector function CollectionEvent_GetFrontPageTimeRemainingCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "frontPageTimeRemainingCol" )
}



string function CollectionEvent_GetFrontTabText( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return ItemFlavor_GetShortName( event )
}



asset function CollectionEvent_GetTabLeftSideImage( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "leftSideImage" )
}



asset function CollectionEvent_GetTabCenterImage( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "centerImage" )
}



asset function CollectionEvent_GetTabRightSideImage( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "rightSideImage" )
}



float function CollectionEvent_GetTabImageSelectedAlpha( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsFloat( ItemFlavor_GetAsset( event ), "imageSelectedAlpha" )
}



float function CollectionEvent_GetTabImageUnselectedAlpha( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsFloat( ItemFlavor_GetAsset( event ), "imageUnselectedAlpha" )
}



asset function CollectionEvent_GetTabCenterRui( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsStringAsAsset( ItemFlavor_GetAsset( event ), "centerRuiAsset" )
}



vector function CollectionEvent_GetTabBGDefaultCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabBGDefaultCol" )
}




vector function CollectionEvent_GetTabBarDefaultCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabBarDefaultCol" )
}




vector function CollectionEvent_GetTabTextDefaultCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabTextDefaultCol" )
}



vector function CollectionEvent_GetTabBGFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabBGFocusedCol" )
}




vector function CollectionEvent_GetTabBarFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabBarFocusedCol" )
}



vector function CollectionEvent_GetTabGlowFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabGlowFocusedCol" )
}




vector function CollectionEvent_GetTabBGSelectedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabBGSelectedCol" )
}




vector function CollectionEvent_GetTabBarSelectedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabBarSelectedCol" )
}



vector function CollectionEvent_GetTabTextSelectedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabTextSelectedCol" )
}



vector function CollectionEvent_GetAboutPageSpecialTextCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "aboutPageSpecialTextCol" )
}



asset function CollectionEvent_GetBGPatternImage( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "bgPatternImage" )
}



asset function CollectionEvent_GetBGTabPatternImage( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "bgTabPatternImage" )
}




asset function CollectionEvent_GetHeaderIcon( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_collection )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "headerIcon" )
}

























































































































int function HeirloomEvent_GetItemCount( ItemFlavor event, bool onlyOwned, entity player = null, bool dontCheckInventoryReady = false )
{
	Assert( dontCheckInventoryReady || !onlyOwned || ( player != null && GRX_IsInventoryReady( player ) ) )

	int count = 0
	array < ItemFlavor > eventItems

	if ( ItemFlavor_GetType( event ) == eItemType.calevent_collection || ItemFlavor_GetType( event ) == eItemType.calevent_milestone )



	{
		eventItems = []
		array<CollectionEventRewardGroup> rewardGroups = CollectionEvent_GetRewardGroups( event )
		foreach ( CollectionEventRewardGroup rewardGroup in rewardGroups )
		{
			foreach ( ItemFlavor reward in rewardGroup.rewards )
			{
				eventItems.append( reward )
			}
		}
	}
	else if ( ItemFlavor_GetType( event ) == eItemType.calevent_themedshop )
	{
		eventItems = GRXPack_GetPackContents( GetItemFlavorByAsset( ThemedShopEvent_GetAssociatedPack( event ) ) )
	}

	foreach ( ItemFlavor item in eventItems )
	{
		









			if ( !onlyOwned || GRX_HasItem( ItemFlavor_GetGRXIndex( item ) ) )
				count++

	}

	return count
}





























































int function HeirloomEvent_GetCurrentRemainingItemCount( ItemFlavor event, entity player )
{
	return HeirloomEvent_GetItemCount( event, false ) - HeirloomEvent_GetItemCount( event, true, player )
}

































































































































































































































































































































































