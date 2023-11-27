

global function ShEventAbilities_Init
global function EventAbilities_GetRewardImage



struct EventAbilityData
{
	int passive = ePassives.INVALID
	bool active = false
	ItemFlavor& calEvent
}




struct FileStruct_LifetimeLevel
{
	table<ItemFlavor, EventAbilityData> eventAbilityDataTable
}




FileStruct_LifetimeLevel fileLevel 










void function ShEventAbilities_Init()
{
	AddCallback_RegisterRootItemFlavors( OnRegisterRootItemFlavors )
	AddCallbackOrMaybeCallNow_OnAllItemFlavorsRegistered( OnAllItemFlavorsRegistered )










}


void function OnAllItemFlavorsRegistered()
{
	foreach ( itemflavor, ead in fileLevel.eventAbilityDataTable )
	{
		if ( !CalEvent_IsActive( ead.calEvent , GetUnixTimestamp()) )
		{
			continue
		}

		ead.active = true
	}
}

void function OnRegisterRootItemFlavors()
{
	foreach ( asset eventAbility in GetBaseItemFlavorsFromArray( "eventAbilities" ) )
	{
		if ( eventAbility == $"" )
			continue

		EventAbilityData ead

		asset calEventAsset = WORKAROUND_AssetAppend( GetGlobalSettingsAsset( eventAbility , "calEventFlav" ), ".rpak" )
		if ( !IsValidItemFlavorSettingsAsset( calEventAsset ) )
		{
			printt("OnRegisterRootItemFlavors skipping registration cause associated cal event not valid")
			continue
		}

		ead.calEvent = GetItemFlavorByAsset( calEventAsset )

		
		
		
		
		


		string passiveRef = GetGlobalSettingsString( eventAbility , "passiveScriptRef" )
		if ( passiveRef == "" )
		{
			printt("OnRegisterRootItemFlavors skipping registration cause passiveRef is empty")
			continue
		}

		Assert( passiveRef in ePassives, "Unknown passive script ref: " + passiveRef )

		ead.passive = ePassives[ passiveRef ]

		ItemFlavor ornull eventAbilityOrNull = RegisterItemFlavorFromSettingsAsset( eventAbility )
		if ( eventAbilityOrNull == null )
			continue

		expect ItemFlavor( eventAbilityOrNull )

		printt("OnRegisterRootItemFlavors registration complete " + string(eventAbility) + " registered")

		fileLevel.eventAbilityDataTable[ eventAbilityOrNull ] <- ead
	}
}

asset function EventAbilities_GetRewardImage( ItemFlavor flavor )
{
	Assert( ItemFlavor_GetType( flavor ) == eItemType.event_ability )

	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( flavor ), "rewardImage" )
}













































      