
global function MilestoneEvents_Init



global function GetActiveMilestoneEvent
global function GetActiveEventTabMilestoneEvent
global function GetActiveStoreOnlyMilestoneEvents
global function GetAllMilestoneEvents
global function IsMilestoneEventStoreOnly
global function GetAllActiveMilestoneEvents
global function MilestoneEvent_GetEventId
global function MilestoneEvent_GetEventById
global function MilestoneEvent_IsEventStoreOnly
global function MilestoneEvent_GetFrontPageRewardBoxTitle
global function MilestoneEvent_GetMainPackFlav
global function MilestoneEvent_IsMilestonePackFlav
global function MilestoneEvent_GetMainPackImage
global function MilestoneEvent_GetFrontPageGRXOfferLocation
global function MilestoneEvent_GetAboutText
global function MilestoneEvent_GetMainIcon
global function MilestoneEvent_GetMilestoneGrantRewards
global function MilestoneEvent_GetStoreEventSectionMainImage
global function MilestoneEvent_GetStoreEventSectionMainText
global function MilestoneEvent_GetStoreEventSectionSubText
global function MilestoneEvent_IsChaseItem
global function MilestoneEvent_IsItemInPool
global function MilestoneEvent_GetEventItems
global function MilestoneEvent_GetEventItemsInPool
global function MilestoneEvent_GetMythicEventItems
global function MilestoneEvent_GetRewardGroups
global function MilestoneEvent_GetGuaranteedPackFlav
global function MilestoneEvent_IsItemEventItem












































































global struct MilestoneEventGrantReward
{
	int         grantingLevel
	ItemFlavor& rewardFlav
	asset 		customImage
	string		customName
}























































































struct FileStruct_LifetimeLevel
{
	EntitySet chasePackGrantQueued
	int milestoneEvents_MilestoneRewardCount = 0
	bool storeOnlyEvents = false
}


FileStruct_LifetimeLevel fileLevel 










void function MilestoneEvents_Init()
{









}



ItemFlavor ornull function GetActiveMilestoneEvent( int t )
{
	Assert( IsItemFlavorRegistrationFinished() )



	
	ItemFlavor ornull event = null
	foreach ( ItemFlavor ev in GetAllItemFlavorsOfType( eItemType.calevent_milestone ) )
	{
		if ( !CalEvent_IsActive( ev, t ) )
			continue

		Assert( event == null, format( "Multiple milestone events are active!! (%s, %s)", string(ItemFlavor_GetAsset( expect ItemFlavor(event) )), string(ItemFlavor_GetAsset( ev )) ) )

		event = ev
	}

	return event





















	unreachable
}










ItemFlavor ornull function GetActiveEventTabMilestoneEvent( int t )
{
	Assert( IsItemFlavorRegistrationFinished() )
	ItemFlavor ornull event = null
	foreach ( ItemFlavor ev in GetAllItemFlavorsOfType( eItemType.calevent_milestone ) )
	{
		if ( !CalEvent_IsActive( ev, t ) )
			continue

		bool isStoreOnly = GetGlobalSettingsBool( ItemFlavor_GetAsset( ev ), "isStoreOnlyMilestoneEvent" )
		if ( isStoreOnly )
			continue

		Assert( event == null, format( "Multiple collection events are active!! (%s, %s)", string(ItemFlavor_GetAsset( expect ItemFlavor(event) )), string(ItemFlavor_GetAsset( ev )) ) )
		event = ev
	}
	return event
}



array<ItemFlavor> function GetActiveStoreOnlyMilestoneEvents( int t )
{
	Assert( IsItemFlavorRegistrationFinished() )
	array<ItemFlavor> activeStoreOnlyMilestoneEvents
	foreach ( ItemFlavor ev in GetAllItemFlavorsOfType( eItemType.calevent_milestone ) )
	{
		if ( !CalEvent_IsActive( ev, t ) )
			continue

		bool isStoreOnly = GetGlobalSettingsBool( ItemFlavor_GetAsset( ev ), "isStoreOnlyMilestoneEvent" )
		if ( !isStoreOnly )
			continue

		activeStoreOnlyMilestoneEvents.append( ev )
	}
	return activeStoreOnlyMilestoneEvents
}



array<ItemFlavor> function GetAllMilestoneEvents( bool includeEventTab = true, bool includeStoreOnly = false )
{
	Assert( IsItemFlavorRegistrationFinished() )
	array<ItemFlavor> storeOnlyMilestoneEvents
	foreach ( ItemFlavor ev in GetAllItemFlavorsOfType( eItemType.calevent_milestone ) )
	{
		bool isStoreOnly = GetGlobalSettingsBool( ItemFlavor_GetAsset( ev ), "isStoreOnlyMilestoneEvent" )

		if ( !includeEventTab && !isStoreOnly )
			continue

		if ( !includeStoreOnly && isStoreOnly )
			continue

		storeOnlyMilestoneEvents.append( ev )
	}
	return storeOnlyMilestoneEvents
}



bool function IsMilestoneEventStoreOnly( ItemFlavor ornull event )
{
	if ( event == null )
		return false

	expect ItemFlavor( event )

	Assert( IsItemFlavorRegistrationFinished() )
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return GetGlobalSettingsBool( ItemFlavor_GetAsset( event ), "isStoreOnlyMilestoneEvent" )
}



array<ItemFlavor> function GetAllActiveMilestoneEvents( int t )
{
	Assert( IsItemFlavorRegistrationFinished() )
	array<ItemFlavor> activeMilestoneEvents

	ItemFlavor ornull activeEvent = GetActiveMilestoneEvent( t )
	if ( activeEvent != null )
	{
		expect ItemFlavor( activeEvent )
		activeMilestoneEvents.append( activeEvent )
	}

	foreach ( ItemFlavor event in GetActiveStoreOnlyMilestoneEvents( t ) )
	{
		activeMilestoneEvents.append( event )
	}

	return activeMilestoneEvents
}



string function MilestoneEvent_GetEventId( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return ItemFlavor_GetGUIDString( event )
}



ItemFlavor ornull function MilestoneEvent_GetEventById( string id, bool activeOnly = false )
{
	Assert( IsItemFlavorRegistrationFinished() )
	Assert( id != "" )
	ItemFlavor ornull event = null
	int unixTimeNow = GetUnixTimestamp()
	foreach ( ItemFlavor ev in GetAllItemFlavorsOfType( eItemType.calevent_milestone ) )
	{
		if ( activeOnly && !CalEvent_IsActive( ev, unixTimeNow ) )
			continue

		if ( MilestoneEvent_GetEventId( ev ) == id )
			return ev
	}
	return null
}



bool function MilestoneEvent_IsEventStoreOnly(  ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return GetGlobalSettingsBool( ItemFlavor_GetAsset( event ), "isStoreOnlyMilestoneEvent" )
}



array<ItemFlavor> function MilestoneEvent_GetLoginRewards( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )

	array<ItemFlavor> rewards = []
	foreach ( var rewardBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), "loginRewards" ) )
	{
		asset rewardAsset = GetSettingsBlockAsset( rewardBlock, "flavor" )
		if ( IsValidItemFlavorSettingsAsset( rewardAsset ) )
			rewards.append( GetItemFlavorByAsset( rewardAsset ) )
	}
	return rewards
}



string function MilestoneEvent_GetFrontPageRewardBoxTitle( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "frontPageRewardBoxTitle" )
}
















































































ItemFlavor function MilestoneEvent_GetMainPackFlav( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return GetItemFlavorByAsset( GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "mainPackFlav" ) )
}



bool function MilestoneEvent_IsMilestonePackFlav( ItemFlavor pack, bool checkEventTab, bool checkStoreOnly )
{
	array<ItemFlavor> milstoneEvents = GetAllMilestoneEvents( checkEventTab, checkStoreOnly )

	foreach ( ItemFlavor event in milstoneEvents )
	{
		ItemFlavor packFlav = MilestoneEvent_GetMainPackFlav( event )
		if ( packFlav == pack )
			return true
	}
	return false
}

















































ItemFlavor function MilestoneEvent_GetGuaranteedPackFlav( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return GetItemFlavorByAsset( GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "guaranteedPackFlav" ) )
}



bool function MilestoneEvent_IsChaseItem( ItemFlavor event, int grxIdx, bool isRestricted = false )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )

	string rewardPath = isRestricted ? "restrictedRewardGroups" : "rewardGroups"
	foreach ( var groupBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), rewardPath ) )
	{
		foreach ( var rewardBlock in IterateSettingsArray( GetSettingsBlockArray( groupBlock, "rewards" ) ) )
		{
			ItemFlavor item = GetItemFlavorByAsset( GetSettingsBlockAsset( rewardBlock, "flavor" ) )
			if ( item.grxIndex == grxIdx )
			{
				return GetSettingsBlockBool( rewardBlock, "milestoneItemIsChaseItem" )
			}
		}
	}
	unreachable
}



bool function MilestoneEvent_IsItemInPool( ItemFlavor event, int grxIdx, bool isRestricted = false )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )

	string rewardPath = isRestricted ? "restrictedRewardGroups" : "rewardGroups"
	foreach ( var groupBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), rewardPath ) )
	{
		foreach ( var rewardBlock in IterateSettingsArray( GetSettingsBlockArray( groupBlock, "rewards" ) ) )
		{
			ItemFlavor item = GetItemFlavorByAsset( GetSettingsBlockAsset( rewardBlock, "flavor" ) )
			if ( item.grxIndex == grxIdx )
			{
				return GetSettingsBlockBool( rewardBlock, "milestoneItemIsInItemPool" )
			}
		}
	}
	unreachable
}























































asset function MilestoneEvent_GetMainPackImage( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "mainPackImage" )
}



string function MilestoneEvent_GetFrontPageGRXOfferLocation( ItemFlavor event, bool isRestricted )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), isRestricted ? "restrictedGRXOfferLocation" : "frontGRXOfferLocation" )
}



array<string> function MilestoneEvent_GetAboutText( ItemFlavor event, bool restricted )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )

	array<string> aboutText = []
	string key              = (restricted ? "aboutTextRestricted" : "aboutTextStandard")
	foreach ( var aboutBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), key ) )
		aboutText.append( GetSettingsBlockString( aboutBlock, "text" ) )
	return aboutText
}



void function MilestoneEvent_GetMainIcon( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
}



array<MilestoneEventGrantReward> function MilestoneEvent_GetMilestoneGrantRewards( ItemFlavor event, bool isRestricted )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )

	array<MilestoneEventGrantReward> groups = []
	string rewardsGroupVar                  = isRestricted ? "restrictedMilestoneGroups" : "milestoneGroups"
	foreach ( var groupBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), rewardsGroupVar ) )
	{
		MilestoneEventGrantReward group
		group.grantingLevel = GetSettingsBlockInt( groupBlock, "milestoneLevel" )
		group.rewardFlav    = GetItemFlavorByAsset( GetSettingsBlockAsset( groupBlock, "reward" ) )
		group.customName 	= GetSettingsBlockString( groupBlock, "milestoneRewardCustomName" )
		group.customImage 	= GetSettingsBlockAsset( groupBlock, "milestoneRewardCustomImage" )
		groups.append( group )
	}
	return groups
}



asset function MilestoneEvent_GetStoreEventSectionMainImage( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "storeEventSectionMainImage" )
}



string function MilestoneEvent_GetStoreEventSectionMainText( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "storeEventSectionMainText" )
}



string function MilestoneEvent_GetStoreEventSectionSubText( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( event ), "storeEventSectionSubText" )
}



















































































































































































array<ItemFlavor> function MilestoneEvent_GetEventItems( ItemFlavor event, bool isRestricted = false )
{
	array<ItemFlavor> chaseItems
	array<ItemFlavor> eventItems
	if ( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	{
		array<CollectionEventRewardGroup> rewardGroups = MilestoneEvent_GetRewardGroups( event, isRestricted )
		foreach ( CollectionEventRewardGroup rewardGroup in rewardGroups )
		{
			
			rewardGroup.rewards.reverse()
			foreach ( ItemFlavor reward in rewardGroup.rewards )
			{
				
				if ( MilestoneEvent_IsChaseItem( event, reward.grxIndex, isRestricted ) )
					chaseItems.append( reward )
				else
					eventItems.append( reward )
			}
		}

		foreach ( ItemFlavor chaseItem in chaseItems )
		{
			eventItems.append( chaseItem )
		}
		chaseItems.clear()
	}
	return eventItems
}



array<ItemFlavor> function MilestoneEvent_GetEventItemsInPool( ItemFlavor event, bool isRestricted = false )
{
	array<ItemFlavor> chaseItems
	array<ItemFlavor> eventItems
	if ( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	{
		array<CollectionEventRewardGroup> rewardGroups = MilestoneEvent_GetRewardGroups( event, isRestricted )
		foreach ( CollectionEventRewardGroup rewardGroup in rewardGroups )
		{
			
			rewardGroup.rewards.reverse()
			foreach ( ItemFlavor reward in rewardGroup.rewards )
			{
				if ( !MilestoneEvent_IsItemInPool( event, reward.grxIndex, isRestricted ) )
					continue

				
				if ( MilestoneEvent_IsChaseItem( event, reward.grxIndex, isRestricted ) )
					chaseItems.append( reward )
				else
					eventItems.append( reward )
			}
		}

		foreach ( ItemFlavor chaseItem in chaseItems )
		{
			eventItems.append( chaseItem )
		}
		chaseItems.clear()
	}
	return eventItems
}



array<ItemFlavor> function MilestoneEvent_GetMythicEventItems( ItemFlavor event, bool isRestricted = false )
{
	array<ItemFlavor> eventItems
	if ( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	{
		array<CollectionEventRewardGroup> rewardGroups = MilestoneEvent_GetRewardGroups( event, isRestricted )
		foreach ( CollectionEventRewardGroup rewardGroup in rewardGroups )
		{
			if ( rewardGroup.quality == eRarityTier.MYTHIC )
			{
				foreach ( ItemFlavor reward in rewardGroup.rewards )
				{
					eventItems.append( reward )
				}
			}
		}
	}
	return eventItems
}




array<CollectionEventRewardGroup> function MilestoneEvent_GetRewardGroups( ItemFlavor event, bool isRestricted = false )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )

	array<CollectionEventRewardGroup> groups = []
	string rewardPath = isRestricted ? "restrictedRewardGroups" : "rewardGroups"
	foreach ( var groupBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), rewardPath ) )
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



bool function MilestoneEvent_IsItemEventItem( ItemFlavor event, int itemIdx )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_milestone )
	array<ItemFlavor> eventItems = MilestoneEvent_GetMythicEventItems( event )
	eventItems.extend( MilestoneEvent_GetEventItems( event, GRX_IsOfferRestricted() ) )
	foreach( ItemFlavor item in eventItems )
	{
		if ( item.grxIndex == itemIdx )
			return true
	}
	return false
}






























































































































































































































































































































































































































































































































































































