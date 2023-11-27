


global function ThemedShopEvents_Init




global function GetActiveThemedShopEvent
global function ThemedShopEvent_GetChallenges
global function ThemedShopEvent_GetHeaderIcon
global function ThemedShopEvent_HasWhatsNew
global function ThemedShopEvent_GetAssociatedPack
global function ThemedShopEvent_HasThemedShopTab
global function ThemedShopEvent_HasSpecialsTab


















































struct FileStruct_LifetimeLevel
{
	table<ItemFlavor, array<ItemFlavor> > eventChallengesMap
}


FileStruct_LifetimeLevel fileLevel 


















void function ThemedShopEvents_Init()
{





	AddCallback_OnItemFlavorRegistered( eItemType.calevent_themedshop, void function( ItemFlavor ev ) {
		fileLevel.eventChallengesMap[ev] <- RegisterReferencedItemFlavorsFromArray( ev, "challenges", "flavor" )
		foreach ( int challengeSortOrdinal, ItemFlavor challengeFlav in fileLevel.eventChallengesMap[ev] )
			RegisterChallengeSource( challengeFlav, ev, challengeSortOrdinal )
	} )
}











ItemFlavor ornull function GetActiveThemedShopEvent( int t )
{
	Assert( IsItemFlavorRegistrationFinished() )
	ItemFlavor ornull event = null
	foreach ( ItemFlavor ev in GetAllItemFlavorsOfType( eItemType.calevent_themedshop ) )
	{
		if ( !CalEvent_IsActive( ev, t ) )
			continue

		Assert( event == null, format( "Multiple themedshop events are active!! (%s, %s)", string(ItemFlavor_GetAsset( expect ItemFlavor(event) )), string(ItemFlavor_GetAsset( ev )) ) )
		event = ev
	}
	return event
}




array<ItemFlavor> function ThemedShopEvent_GetChallenges( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_themedshop )

	return fileLevel.eventChallengesMap[event]
}





















































































































































































































bool function ThemedShopEvent_HasWhatsNew( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_themedshop )
	return GetGlobalSettingsBool( ItemFlavor_GetAsset( event ), "whatsNewTab" )
}



asset function ThemedShopEvent_GetAssociatedPack( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_themedshop )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "associatedPackFlav" )
}



bool function ThemedShopEvent_HasThemedShopTab( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_themedshop )
	return GetGlobalSettingsBool( ItemFlavor_GetAsset( event ), "themedShopTab" )
}



bool function ThemedShopEvent_HasSpecialsTab( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_themedshop )
	return GetGlobalSettingsBool( ItemFlavor_GetAsset( event ), "specialsTab" )
}





































asset function ThemedShopEvent_GetHeaderIcon( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_themedshop )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( event ), "headerIcon" )
}








































