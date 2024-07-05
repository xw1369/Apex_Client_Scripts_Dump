
global function TimeGatedLoginRewards_Init
global function LoginEvent_GetLoginRewards




struct FileStruct_LifetimeLevel
{



}




FileStruct_LifetimeLevel fileLevel 










void function TimeGatedLoginRewards_Init()
{









}




array<ItemFlavor> function GetActiveLoginEvents( int t )
{
	Assert( IsItemFlavorRegistrationFinished() )
	array<ItemFlavor> activeEvents
	foreach ( ItemFlavor ev in GetAllItemFlavorsOfType( eItemType.calevent_login ) )
	{
		if ( !CalEvent_IsActive( ev, t ) )
			continue

		activeEvents.append( ev )

	}

	return activeEvents
}

































































































































array<ItemFlavor> function LoginEvent_GetLoginRewards( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_login )

	array<ItemFlavor> rewards = []
	foreach ( var rewardBlock in IterateSettingsAssetArray( ItemFlavor_GetAsset( event ), "loginRewards" ) )
	{
		asset rewardAsset = GetSettingsBlockAsset( rewardBlock, "flavor" )
		if ( IsValidItemFlavorSettingsAsset( rewardAsset ) )
			rewards.append( GetItemFlavorByAsset( rewardAsset ) )
	}
	return rewards
}













