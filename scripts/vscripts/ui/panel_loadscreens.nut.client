
global function UpdateLoadscreenPreviewMaterial
global function ClLoadscreensInit






struct
{











		array<PakHandle> pakHandles

} file


void function ClLoadscreensInit()
{
	RegisterSignal( "UpdateLoadscreenPreviewMaterial" )
}


































































































































void function UpdateLoadscreenPreviewMaterial( var loadscreenElem, var descriptionElem, SettingsAssetGUID guid )
{
	
	thread UpdateLoadscreenPreviewMaterial_internal( loadscreenElem, descriptionElem, guid )
}

void function UpdateLoadscreenPreviewMaterial_internal( var loadscreenElem, var descriptionElem, SettingsAssetGUID guid )
{
	Signal( clGlobal.signalDummy, "UpdateLoadscreenPreviewMaterial" )
	EndSignal( clGlobal.signalDummy, "UpdateLoadscreenPreviewMaterial" )

	
	RuiSetImage( Hud_GetRui( loadscreenElem ), "loadscreenImage", $"" )
	RuiSetBool( Hud_GetRui( loadscreenElem ), "loadscreenImageIsReady", false )
	Hud_SetVisible( loadscreenElem, false )
	if ( descriptionElem != null )
	{
		
		Hud_SetText( descriptionElem, "" )
		Hud_SetVisible( descriptionElem, false )
	}

	OnThreadEnd(
		function() : ()
		{
			
			foreach( handle in file.pakHandles )
			{
				if ( handle.isAvailable )
					ReleasePakFile( handle )
			}
		}
	)

	if ( guid == 0 )
		return

	
	ItemFlavor flavor = GetItemFlavorByGUID( guid )
	Assert( ItemFlavor_GetType( flavor ) == eItemType.loadscreen )
	asset loadscreenImage = Loadscreen_GetLoadscreenImageAsset( flavor )

	if ( loadscreenImage == $"" )
		return

	WaitFrame() 





	Hud_SetVisible( loadscreenElem, true )

	
	string rpak         = Loadscreen_GetRPakName( flavor )
	PakHandle pakHandle = RequestPakFile( rpak, TRACK_FEATURE_UI )
	file.pakHandles.append( pakHandle )

	if ( !pakHandle.isAvailable )
		WaitSignal( pakHandle, "PakFileLoaded" )

	RuiSetImage( Hud_GetRui( loadscreenElem ), "loadscreenImage", loadscreenImage )
	RuiSetBool( Hud_GetRui( loadscreenElem ), "loadscreenImageIsReady", true )
	Hud_SetVisible( loadscreenElem, true )

	if ( descriptionElem != null )
	{
		string description = Loadscreen_GetImageOverlayText( flavor )
		if ( description != "" )
		{
			
			Hud_SetText( descriptionElem, description )
			Hud_SetVisible( descriptionElem, true )
		}
	}

	WaitForever()
}

