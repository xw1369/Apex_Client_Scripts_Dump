
global function MilestoneEvents_Init



global function GetActiveMilestoneEvent
global function MilestoneEvent_GetFrontPageRewardBoxTitle
global function MilestoneEvent_GetMainPackFlav
global function MilestoneEvent_GetMainPackImage
global function MilestoneEvent_GetFrontPageGRXOfferLocation
global function MilestoneEvent_GetAboutText
global function MilestoneEvent_GetMainIcon
global function MilestoneEvent_GetMilestoneGrantRewards





































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

		Assert( event == null, format( "Multiple collection events are active!! (%s, %s)", string(ItemFlavor_GetAsset( expect ItemFlavor(event) )), string(ItemFlavor_GetAsset( ev )) ) )
		event = ev
	}
	return event
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













































































































































































































































































































































































































