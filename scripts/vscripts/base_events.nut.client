
global function BaseEvents_Init



global function GetActiveBaseEvent















global struct RTKBaseEventLandingTitleData
{
	string		text
	int 		pixelHeight
	vector		color
	float		outlineWidth
}

global struct RTKBaseEventLandingSubtitleData
{
	int 		pixelHeight
	vector		color
}

global enum eEventLandingPageButtonRedirect
{
	LANDING = 0
	MILESTONES = 1
	COLLECTION = 2
	EVENT_SHOP = 3
	COLLECTION_EVENT = 4
	EVENT_STORE = 5
	PLAY_MODE = 6
	VGUI_PRIZE_TRACKER = 7 
	CUSTOM_DEEPLINK = 8
}















struct FileStruct_LifetimeLevel
{
	EntitySet chasePackGrantQueued
}



FileStruct_LifetimeLevel fileLevel 
















void function BaseEvents_Init()
{




}



ItemFlavor ornull function GetActiveBaseEvent( int t )
{
	Assert( IsItemFlavorRegistrationFinished() )
	ItemFlavor ornull event = null
	foreach ( ItemFlavor ev in GetAllItemFlavorsOfType( eItemType.calevent_base_event ) )
	{
		if ( !CalEvent_IsActive( ev, t ) )
			continue

		Assert( event == null, format( "Multiple base events are active!! (%s, %s)", string(ItemFlavor_GetAsset( expect ItemFlavor(event) )), string(ItemFlavor_GetAsset( ev )) ) )
		event = ev
	}
	return event
}

















































































































































