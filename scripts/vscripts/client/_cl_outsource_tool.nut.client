global function ServerCallback_OVUpdateModelBounds
global function ServerCallback_OVToggle

global function ClientCodeCallback_ResetCamera
global function ClientCodeCallback_ResetModel
global function ClientCodeCallback_SwitchOutsourceEnv
global function ClientCodeCallback_UpdateOutsourceModel
global function ClientCodeCallback_SetAxisLockedFlags
global function ClientCodeCallback_UpdateMousePos
global function ClientCodeCallback_InputMouseScrolledUp
global function ClientCodeCallback_InputMouseScrolledDown
global function ClientCodeCallback_SetMoveModel
global function ClientCodeCallback_ToggleModelInShadow
global function ClientCodeCallback_UpdateOutsourceBaseModel
global function ClientCodeCallback_ToggleOutsourceCubemapSpheres
global function ClientCodeCallback_ToggleCharacterSelectMenu
global function ClientCodeCallback_ToggleOutsourceGladCard

#if DEV
global function OutsourceViewer_IsActive

const string OUTSOURCE_VIEWER_NODES_SCRIPT_NAME = "Outsource_Viewer_Node"
const string OUTSOURCE_INFO_TARGET_CLASS_NAME = "info_target_clientside"

const int AXIS_LOCKED_X	= ( 1 << 0 )
const int AXIS_LOCKED_Y	= ( 1 << 1 )

const float SUN_PITCH = 30.0

const vector WEAPON_FOCUS_OFFSET = < 0.0, 0.0, 3.0 >
const vector CHARACTER_FOCUS_OFFSET = < 0, 0, 20.0 >

const vector WEAPON_ROTATION_OFFSET = < 0.0, -90.0, 0.0 >
const vector CHARM_ROTATION_OFFSET = < 0.0, -90.0, 0.0 >

const float VIEWER_CHARM_SIZE = 10.0

const float SIDEBYSIDE_DIST_SCALE = 1.4

const float STICK_DEADZONE = 0.1
const float STICK_ROTATE_SPEED = 2.0
const float STICK_MOVE_SPEED = 0.5

const float MOUSE_MOVE_SPEED = 0.1

const float BOUNDING_BOX_HEIGHT = 140
const float BOUNDING_BOX_RADIUS = 19600

const float MAX_DIST_FROM_FOCUS = 1200.0

const float CHARACTER_BASE_ZOOM = 120
const float WEAPON_BASE_ZOOM = 60
const float CHARM_BASE_ZOOM = 42

const float MODEL_FADE_DIST = 9999999

const float halfPi = PI / 2.0


const int RUI_NAME_PLATE_WIDTH = 262
const int RUI_NAME_PLATE_HEIGHT = 110

const asset BACKGROUND_GEO_MODEL = $"mdl/levels_terrain/mp_lobby/mp_character_select_geo.rmdl" 
const asset BACKGROUND_SMOKE_MODEL = $"mdl/levels_terrain/mp_lobby/mp_character_select_smoke.rmdl" 
const asset LIGHT_RIG_MDL = $"mdl/empty/lights/empty_lights.rmdl"
const asset CHARACTER_SELECT_VIGNETTE = $"rui/menu/common/menu_vignette"

const string CAMERA_ENT_NAME = "target_char_sel_camera_new"
const string CHARACTER_ENT_NAME = "target_char_sel_pilot_new"
const string BACKGROUND_ENT_NAME = "target_char_sel_bg_new"

const string LIGHT_KEY_NAME = "char_sel_light_key"
const string LIGHT_FILL_NAME = "char_sel_light_fill"
const string LIGHT_RIML_NAME = "char_sel_light_rim_l"
const string LIGHT_RIMR_NAME = "char_sel_light_rim_r"

const float CHARACTER_SELECT_CAMERA_FOV = 35.5

struct spawnNode
{
	vector pos
	vector ang
}

struct {
	bool characterSelectMenuInitialized

	entity backgroundModelGeo
	entity backgroundModelSmoke
	var backgroundRuiTopo
	var backgroundRui
	var scrollingBGRui
	var vignetteRui

	entity lightRigModel
	entity keyLight
	entity fillLight
	entity rimLightL
	entity rimLightR

	entity camera
	entity modelNode

	bool restoreShowBaseSkin
	bool restoreShowBaseSkinSideBySide
	vector restoreViewModelPos
	vector restoreViewModelAng
	int restoreViewModelNode
} characterSelectMenu

struct {
	bool outsourceViewerRunning = false

	entity viewerModel
	entity viewerBaseSkinModel
	entity viewerCharmModel
	entity modelMover
	entity viewerBaseSkinModelMover
	vector modelMoverRealOrigin

	array<ItemFlavor> availableAssets
	array<ItemFlavor> availableAssetSkins
	array<ItemFlavor> availableCharms

	bool useWorldModel = false
	bool showBaseSkin = false
	bool showBaseSkinSideBySide = false
	int currentAssetType = eAssetType.ASSETTYPE_CHARACTER
	int currentAsset = 0
	int currentAssetSkin = 0
	int currentCharm = 0

	table<string, vector> modelBounds = { mins = <0, 0, 0>, maxs = <0, 0, 0> }
	bool modelSelectionBoundsVisible = false
	bool hasCurrentModelBounds = false

	bool tumbleModeActive = true
	int axisLockedFlags = 0

	float screenWidth = 1920 
	float screenHeight = 1080 
	float[2] previousMousePos	= [0, 0]
	float[2] mouseDelta			= [0, 0]
	float[2] rotationDelta		= [0, 0]
	float[2] rotationVel		= [0, 0]
	float[2] movementDelta		= [0, 0]

	array<spawnNode> spawnNodes
	int currentNode = 0
	int previousNode = 0

	bool shouldBeInShadow = false

	bool watermarkEnabled = false
	var watermarkRui

	NestedGladiatorCardHandle& nestedGCHandle
	var gladCardRui

	entity viewerCameraEntity
	float viewerCameraFOV
	entity cubemapSpheres

	float mouseWheelNewValue
	float mouseWheelLastValue

	bool showCharacterSelectMenu = false
	bool shouldResetCameraOnModelUpdate = true
	float maxZoomIncrement_Mouse = 60.0
	float maxZoomIncrement_Controller = 20.0
	float lastZoomVal = 0.0
	float maxZoomAmount
	vector zoomTrackStartPos
	vector zoomTrackEndPos
	vector zoomNormVec
} file

bool function OutsourceViewer_IsActive()
{
	return file.outsourceViewerRunning
}

void function EnableViewerWatermark()
{
	if ( file.watermarkEnabled )
		return

	file.watermarkEnabled = true

	var watermarkRui = RuiCreate( MINIMAP_UID_COORDS_RUI, clGlobal.topoFullScreen, RUI_DRAW_HUD, MINIMAP_Z_BASE + 1000 )
	InitHUDRui( watermarkRui )

	float watermarkTextScale = GetCurrentPlaylistVarFloat( "watermark_text_scale", 1.0 )
	float watermarkAlphaScale = GetCurrentPlaylistVarFloat( "watermark_alpha_scale", 1.0 )
	RuiSetFloat( watermarkRui, "watermarkTextScale", watermarkTextScale )
	RuiSetFloat( watermarkRui, "watermarkAlphaScale", watermarkAlphaScale )

	string uidString = GetConVarString( "platform_user_id" )
	if ( IsOdd( uidString.len() ) )
		uidString = "0" + uidString
	int uidLength     = uidString.len()
	int uidHalfLength = uidLength / 2
	string uidPart1   = uidString.slice( 0, uidHalfLength )
	string uidPart2   = uidString.slice( uidHalfLength, uidLength )
	Assert( uidPart1.len() == uidPart2.len() )

	string fakeHexUidString = GetUIDHex()
	RuiSetString( watermarkRui, "uid", fakeHexUidString )
	RuiSetInt( watermarkRui, "uidPart1", int( uidPart1 ) )
	RuiSetInt( watermarkRui, "uidPart2", int( uidPart2 ) )

	RuiSetString( watermarkRui, "name", GetPlayerName() )
	RuiSetBool( watermarkRui, "alwaysOn", true )
	RuiSetVisible( watermarkRui, true )

	file.watermarkRui = watermarkRui
}

void function DisableViewerWatermark()
{
	file.watermarkEnabled = false
	RuiDestroyIfAlive( file.watermarkRui )
}

void function InitCharacterSelectMenu()
{
	if ( characterSelectMenu.characterSelectMenuInitialized )
		return

	entity cameraNode = GetEntByScriptName( CAMERA_ENT_NAME )
	vector cameraOrigin = cameraNode.GetOrigin()


	characterSelectMenu.camera = CreateClientSidePointCamera( cameraOrigin, cameraNode.GetAngles() + <0, 8.751, 0>, CHARACTER_SELECT_CAMERA_FOV )




	characterSelectMenu.modelNode = GetEntByScriptName( CHARACTER_ENT_NAME )

	if ( IsValid( characterSelectMenu.backgroundModelGeo ) )
		characterSelectMenu.backgroundModelGeo.Destroy()

	entity backgroundNode = GetEntByScriptName( BACKGROUND_ENT_NAME )
	vector backgroundNodeOffsetOrigin = backgroundNode.GetOrigin() -< 37.5, -7, 24>
	vector backgroundNodeOffsetAngles = backgroundNode.GetAngles()

	backgroundNodeOffsetAngles += <0, 8.751, 0>


	characterSelectMenu.backgroundModelGeo = CreateClientSidePropDynamic( backgroundNodeOffsetOrigin, backgroundNodeOffsetAngles, BACKGROUND_GEO_MODEL )
	characterSelectMenu.backgroundModelGeo.kv.solid = 0
	characterSelectMenu.backgroundModelGeo.kv.disableshadows = 1
	characterSelectMenu.backgroundModelGeo.kv.fadedist = -1
	characterSelectMenu.backgroundModelGeo.MakeSafeForUIScriptHack()

	if ( IsValid( characterSelectMenu.backgroundModelSmoke ) )
		characterSelectMenu.backgroundModelSmoke.Destroy()

	characterSelectMenu.backgroundModelSmoke = CreateClientSidePropDynamic( backgroundNodeOffsetOrigin, backgroundNodeOffsetAngles, BACKGROUND_SMOKE_MODEL )
	characterSelectMenu.backgroundModelSmoke.kv.solid = 0
	characterSelectMenu.backgroundModelSmoke.kv.disableshadows = 1
	characterSelectMenu.backgroundModelSmoke.kv.fadedist = -1
	characterSelectMenu.backgroundModelSmoke.MakeSafeForUIScriptHack()

	characterSelectMenu.lightRigModel = CreateClientSidePropDynamic( backgroundNode.GetOrigin(), backgroundNodeOffsetAngles, LIGHT_RIG_MDL )
	characterSelectMenu.lightRigModel.MakeSafeForUIScriptHack()
	characterSelectMenu.lightRigModel.SetParent( backgroundNode )
	characterSelectMenu.lightRigModel.Hide()

	characterSelectMenu.keyLight = GetEntByScriptName( LIGHT_KEY_NAME )
	characterSelectMenu.keyLight.SetTweakLightRealtimeShadows( true )
	characterSelectMenu.keyLight.SetTweakLightUpdateShadowsEveryFrame( true )

	characterSelectMenu.fillLight = GetEntByScriptName( LIGHT_FILL_NAME )
	characterSelectMenu.fillLight.SetTweakLightRealtimeShadows( true )
	characterSelectMenu.fillLight.SetTweakLightUpdateShadowsEveryFrame( true )

	characterSelectMenu.rimLightL = GetEntByScriptName( LIGHT_RIML_NAME )
	characterSelectMenu.rimLightL.SetTweakLightRealtimeShadows( true )
	characterSelectMenu.rimLightL.SetTweakLightUpdateShadowsEveryFrame( true )

	characterSelectMenu.rimLightR = GetEntByScriptName( LIGHT_RIMR_NAME )
	characterSelectMenu.rimLightR.SetTweakLightRealtimeShadows( true )
	characterSelectMenu.rimLightR.SetTweakLightUpdateShadowsEveryFrame( true )

	
	vector backgroundRuiOrigin = cameraOrigin + ( cameraNode.GetForwardVector() * 200 )

	vector backgroundRuiAngles = characterSelectMenu.modelNode.GetAngles() + <0, -8.751, 0>




	characterSelectMenu.backgroundRuiTopo = CreateRUITopology_Worldspace( backgroundRuiOrigin, backgroundRuiAngles * -1, RUI_NAME_PLATE_WIDTH, RUI_NAME_PLATE_HEIGHT )

	characterSelectMenu.scrollingBGRui = RuiCreate( $"ui/character_select_scrolling_bg.rpak", characterSelectMenu.backgroundRuiTopo, RUI_DRAW_WORLD, 0 )
	characterSelectMenu.backgroundRui = CreateFullscreenRui( $"ui/character_select_class_name_plate.rpak", 300 )




	characterSelectMenu.vignetteRui = RuiCreate( $"ui/basic_image.rpak", clGlobal.topoFullScreen, RUI_DRAW_HUD, MINIMAP_Z_BASE )
	RuiSetImage( characterSelectMenu.vignetteRui, "basicImage", CHARACTER_SELECT_VIGNETTE )
	RuiSetVisible( characterSelectMenu.vignetteRui, false )

	characterSelectMenu.characterSelectMenuInitialized = true
}

void function DestroyCharacterSelectMenu()
{
	if ( IsValid( characterSelectMenu.backgroundModelGeo ) )
		characterSelectMenu.backgroundModelGeo.Destroy()
	if ( IsValid( characterSelectMenu.backgroundModelSmoke ) )
		characterSelectMenu.backgroundModelSmoke.Destroy()
	if ( IsValid( characterSelectMenu.lightRigModel ) )
		characterSelectMenu.lightRigModel.Destroy()
	if ( IsValid( characterSelectMenu.camera ) )
		characterSelectMenu.camera.Destroy()

	if ( characterSelectMenu.backgroundRuiTopo != null )
	{
		RuiTopology_Destroy( characterSelectMenu.backgroundRuiTopo )
		characterSelectMenu.backgroundRuiTopo = null
	}
	if ( characterSelectMenu.backgroundRui != null )
	{
		RuiDestroyIfAlive( characterSelectMenu.backgroundRui )
		characterSelectMenu.backgroundRui = null
	}
	if ( characterSelectMenu.scrollingBGRui != null )
	{
		RuiDestroyIfAlive( characterSelectMenu.scrollingBGRui )
		characterSelectMenu.scrollingBGRui = null
	}
	if ( characterSelectMenu.vignetteRui != null )
	{
		RuiDestroyIfAlive( characterSelectMenu.vignetteRui )
		characterSelectMenu.vignetteRui = null
	}

	characterSelectMenu.characterSelectMenuInitialized = false
}

void function UpdateCharacterSelectLightingAndBackground( ItemFlavor characterSkin )
{
	ItemFlavor character = CharacterSkin_GetCharacterFlavor( characterSkin )

	
	int backgroundSkinIndex = CharacterClass_GetMenuBackgroundSkinIndex( character )
	if ( IsValid( characterSelectMenu.backgroundModelGeo ) )
		characterSelectMenu.backgroundModelGeo.SetSkin( backgroundSkinIndex )

	
	int smokeSkinIndex = CharacterClass_GetMenuSmokeSkinIndex( character )
	if ( IsValid( characterSelectMenu.backgroundModelSmoke ) )
		characterSelectMenu.backgroundModelSmoke.SetSkin( smokeSkinIndex )

	CharacterMenuLightData lightData
	if ( CharacterSkin_HasMenuCustomLighting( characterSkin ) )
	{
		lightData = CharacterSkin_GetMenuCustomLightData( characterSkin )
	}
	else
	{
		lightData = CharacterClass_GetMenuLightData( character )
	}

	characterSelectMenu.keyLight.SetTweakLightColor( lightData.key_color )
	characterSelectMenu.keyLight.SetTweakLightBrightness( lightData.key_brightness )
	characterSelectMenu.keyLight.SetTweakLightCone( lightData.key_cone )
	characterSelectMenu.keyLight.SetTweakLightInnerCone ( lightData.key_innercone )
	characterSelectMenu.keyLight.SetTweakLightDistance( lightData.key_distance )
	characterSelectMenu.keyLight.SetTweakLightHalfBrightFrac( lightData.key_halfbrightfrac )
	characterSelectMenu.keyLight.SetTweakLightSpecIntensity( lightData.key_specint )
	characterSelectMenu.keyLight.SetTweakLightPBRFalloff( lightData.key_pbrfalloff )
	characterSelectMenu.keyLight.SetTweakLightRealtimeShadows( lightData.key_castshadows )
	characterSelectMenu.fillLight.SetTweakLightColor( lightData.fill_color )
	characterSelectMenu.fillLight.SetTweakLightBrightness( lightData.fill_brightness )
	characterSelectMenu.fillLight.SetTweakLightCone( lightData.fill_cone )
	characterSelectMenu.fillLight.SetTweakLightInnerCone ( lightData.fill_innercone )
	characterSelectMenu.fillLight.SetTweakLightDistance( lightData.fill_distance )
	characterSelectMenu.fillLight.SetTweakLightHalfBrightFrac( lightData.fill_halfbrightfrac )
	characterSelectMenu.fillLight.SetTweakLightSpecIntensity( lightData.fill_specint )
	characterSelectMenu.fillLight.SetTweakLightPBRFalloff( lightData.fill_pbrfalloff )
	characterSelectMenu.fillLight.SetTweakLightRealtimeShadows( lightData.fill_castshadows )
	characterSelectMenu.rimLightL.SetTweakLightColor( lightData.rimL_color )
	characterSelectMenu.rimLightL.SetTweakLightBrightness( lightData.rimL_brightness )
	characterSelectMenu.rimLightL.SetTweakLightCone( lightData.rimL_cone )
	characterSelectMenu.rimLightL.SetTweakLightInnerCone ( lightData.rimL_innercone )
	characterSelectMenu.rimLightL.SetTweakLightDistance( lightData.rimL_distance )
	characterSelectMenu.rimLightL.SetTweakLightHalfBrightFrac( lightData.rimL_halfbrightfrac )
	characterSelectMenu.rimLightL.SetTweakLightSpecIntensity( lightData.rimL_specint )
	characterSelectMenu.rimLightL.SetTweakLightPBRFalloff( lightData.rimL_pbrfalloff )
	characterSelectMenu.rimLightL.SetTweakLightRealtimeShadows( lightData.rimL_castshadows )
	characterSelectMenu.rimLightR.SetTweakLightColor( lightData.rimR_color )
	characterSelectMenu.rimLightR.SetTweakLightBrightness( lightData.rimR_brightness )
	characterSelectMenu.rimLightR.SetTweakLightCone( lightData.rimR_cone )
	characterSelectMenu.rimLightR.SetTweakLightInnerCone ( lightData.rimR_innercone )
	characterSelectMenu.rimLightR.SetTweakLightDistance( lightData.rimR_distance )
	characterSelectMenu.rimLightR.SetTweakLightHalfBrightFrac( lightData.rimR_halfbrightfrac )
	characterSelectMenu.rimLightR.SetTweakLightSpecIntensity( lightData.rimR_specint )
	characterSelectMenu.rimLightR.SetTweakLightPBRFalloff( lightData.rimR_pbrfalloff )
	characterSelectMenu.rimLightR.SetTweakLightRealtimeShadows( lightData.rimR_castshadows )

	characterSelectMenu.lightRigModel.Anim_Play( lightData.animSeq )
	int attachIdx_fill = characterSelectMenu.lightRigModel.LookupAttachment( "LIGHT_1" )
	int attachIdx_rimL = characterSelectMenu.lightRigModel.LookupAttachment( "LIGHT_2" )
	int attachIdx_rimR = characterSelectMenu.lightRigModel.LookupAttachment( "LIGHT_3" )
	int attachIdx_key = characterSelectMenu.lightRigModel.LookupAttachment( "LIGHT_4" )

	characterSelectMenu.fillLight.SetTweakLightOrigin( characterSelectMenu.lightRigModel.GetAttachmentOrigin( attachIdx_fill ) )
	characterSelectMenu.fillLight.SetTweakLightAngles( characterSelectMenu.lightRigModel.GetAttachmentAngles( attachIdx_fill ) )

	characterSelectMenu.rimLightL.SetTweakLightOrigin( characterSelectMenu.lightRigModel.GetAttachmentOrigin( attachIdx_rimL ) )
	characterSelectMenu.rimLightL.SetTweakLightAngles( characterSelectMenu.lightRigModel.GetAttachmentAngles( attachIdx_rimL ) )

	characterSelectMenu.rimLightR.SetTweakLightOrigin( characterSelectMenu.lightRigModel.GetAttachmentOrigin( attachIdx_rimR ) )
	characterSelectMenu.rimLightR.SetTweakLightAngles( characterSelectMenu.lightRigModel.GetAttachmentAngles( attachIdx_rimR ) )

	characterSelectMenu.keyLight.SetTweakLightOrigin( characterSelectMenu.lightRigModel.GetAttachmentOrigin( attachIdx_key ) )
	characterSelectMenu.keyLight.SetTweakLightAngles( characterSelectMenu.lightRigModel.GetAttachmentAngles( attachIdx_key ) )

	Chroma_SetBaseLayer( CHROMALOOP_SMOKE, CHROMATRANS_VERTICAL, CharacterClass_GetChromaGradient( character ), 0.5 )


	RuiSetString( characterSelectMenu.scrollingBGRui, "nameString", Localize( ItemFlavor_GetLongName( character ) ) )
	RuiSetString( characterSelectMenu.scrollingBGRui, "footnoteString", Localize( ItemFlavor_GetShortDescription( character ) ) )




}

void function InitViewerGladiatorCard()
{
	file.gladCardRui = CreateFullscreenRui( $"ui/spectator_card.rpak", RUI_SORT_GLADCARD )
	file.nestedGCHandle = CreateNestedGladiatorCard( file.gladCardRui, "cardFrontFace", eGladCardDisplaySituation.GAME_INTRO_MY_SQUAD_ANIMATED, eGladCardPresentation.FULL_BOX )
	ChangeNestedGladiatorCardOwner( file.nestedGCHandle, LocalClientEHI() )
	RuiSetVisible( file.gladCardRui, false )
}

void function DestroyViewerGladiatorCard()
{
	CleanupNestedGladiatorCard( file.nestedGCHandle )
	RuiDestroyIfAlive( file.gladCardRui )
}

void function RefreshAssetItemFlavorsForType( int type )
{
	file.availableAssets.clear()
	if ( IsValidItemFlavorType( type, eValidation.ASSERT ) )
		file.availableAssets = clone GetAllItemFlavorsOfType( type )

	Assert( file.availableAssets.len() > 0 )
	if ( file.availableAssets.len() == 0 )
	{
		Warning( "Art Viewer - No item flavors found on refresh for type %i.", type )
		file.currentAsset = 0
	}
	else if ( file.currentAsset >= file.availableAssets.len() )
	{
		Warning( "Art Viewer - Current asset index out of bounds (current: %i, max: %i), setting to 0.", file.currentAsset, file.availableAssets.len() )
		file.currentAsset = 0
	}

	array< string > availableAssetNames
	foreach ( assetFlavor in file.availableAssets )
	{
		string assetName = Localize( ItemFlavor_GetLongName( assetFlavor ) )
		availableAssetNames.append( assetName )
	}

	UpdateOVAssetUI( ARTVIEWER_PROPERTIES_ASSET, availableAssetNames, file.currentAsset )

	if ( file.currentAssetType == eAssetType.ASSETTYPE_WEAPON )
		UpdateCurrentWeaponClassname();
}

void function RefreshSkinItemFlavorsForCurrentAsset()
{
	file.availableAssetSkins.clear()
	if ( file.availableAssets.len() > 0 ) 
	{
		ItemFlavor currentAsset = file.availableAssets[ file.currentAsset ]
		int type = ItemFlavor_GetType( currentAsset )
		if ( IsValidItemFlavorType( type, eValidation.ASSERT ) )
		{
			switch ( ItemFlavor_GetType( currentAsset ) )
			{
				case eItemType.character:
					file.availableAssetSkins = clone GetValidItemFlavorsForLoadoutSlot( EHI_null, Loadout_CharacterSkin( currentAsset ) )
					break
				case eItemType.loot_main_weapon:
					file.availableAssetSkins = clone GetValidItemFlavorsForLoadoutSlot( EHI_null, Loadout_WeaponSkin( currentAsset ) )
					break
				default:
					Warning( "Art Viewer - Unhandled flavor type" )
			}
		}

		if ( file.availableAssetSkins.len() > 0 )
		{
			if ( file.currentAssetSkin >= file.availableAssetSkins.len() )
			{
				Warning( "Art Viewer - Current skin index out of bounds (current: %i, max: %i), setting to 0.", file.currentAssetSkin, file.availableAssetSkins.len() )
				file.currentAssetSkin = 0
			}
			file.availableAssetSkins.remove( 0 ) 
		}
		else
		{
			Warning( "Art Viewer - No skins found for %s on refresh.", ItemFlavor_GetLongName( currentAsset ) )
			file.currentAssetSkin = 0
		}
	}

	array< OutsourceViewer_SkinDetails > currentAssetSkinNamesAndTiers
	foreach ( skinFlavor in file.availableAssetSkins )
	{
		OutsourceViewer_SkinDetails skinDetails
		skinDetails.skinName = Localize( ItemFlavor_GetLongName( skinFlavor ) )
		skinDetails.skinAssetName = ItemFlavor_GetAsset( skinFlavor )
		skinDetails.skinTier = ItemFlavor_GetQuality( skinFlavor, 0 )
		currentAssetSkinNamesAndTiers.append( skinDetails )
	}

	UpdateOVAssetUI( ARTVIEWER_PROPERTIES_SKIN, currentAssetSkinNamesAndTiers, file.currentAssetSkin )
}

void function RefreshCharmItemFlavors()
{
	file.availableCharms.clear()
	file.availableCharms = clone GetAllItemFlavorsOfType( eItemType.weapon_charm )

	Assert( file.availableCharms.len() > 0 )
	if ( file.availableCharms.len() == 0 )
	{
		Warning( "Art Viewer - No charms found on item flavor refresh." )
		file.currentCharm = 0
	}
	else if ( file.currentCharm >= file.availableCharms.len() )
	{
		Warning( "Art Viewer - Current charm index out of bounds (current: %i, max: %i), setting to 0.", file.currentCharm, file.availableCharms.len() )
		file.currentCharm = 0
	}

	array< string > availableCharmNames
	foreach ( charmFlavor in file.availableCharms )
	{
		string charmName = Localize( ItemFlavor_GetLongName( charmFlavor ) )
		availableCharmNames.append( charmName )
	}

	UpdateOVAssetUI( ARTVIEWER_PROPERTIES_CHARM, availableCharmNames, file.currentCharm )
}

void function RefreshAssetItemFlavors()
{
	switch ( file.currentAssetType )
	{
		case eAssetType.ASSETTYPE_CHARACTER:
			RefreshAssetItemFlavorsForType( eItemType.character )
			RefreshSkinItemFlavorsForCurrentAsset()
			break
		case eAssetType.ASSETTYPE_WEAPON:
			RefreshAssetItemFlavorsForType( eItemType.loot_main_weapon )
			RefreshSkinItemFlavorsForCurrentAsset()
			RefreshCharmItemFlavors()
			break
		case eAssetType.ASSETTYPE_CHARM:
			file.currentAsset = file.currentCharm 
			RefreshAssetItemFlavorsForType( eItemType.weapon_charm )
			break
		default:
			Warning( "Art Viewer - Unknown asset type: ", file.currentAssetType  )
			break
	}
}

void function ToggleTumbleMode()
{
	file.tumbleModeActive = !file.tumbleModeActive
}

void function ShowModelSectionBounds()
{
	while ( file.modelSelectionBoundsVisible )
	{
		vector mins = file.modelBounds.mins
		vector maxs = file.modelBounds.maxs

		int r = 160
		int g = 160
		int b = 0
		DrawAngledBox( file.viewerModel.GetOrigin(), file.viewerModel.GetAngles(), mins, maxs, <r, g, b>, true, 0.02 )

		vector originMin = < -0.1, -0.1, -0.1 >
		vector originMax = < 0.1, 0.1, 0.1 >
		DrawAngledBox( file.viewerModel.GetOrigin(), file.viewerModel.GetAngles(), originMin, originMax, COLOR_RED, true, 0.02 )

		DrawAngledBox( ArtViewer_GetEntityOrigin(), file.modelMover.GetAngles(), originMin, originMax, COLOR_GREEN, true, 0.02 )
		wait( 0.0 )
	}
}

void function ToggleModelSelectionBounds()
{
	file.modelSelectionBoundsVisible = !file.modelSelectionBoundsVisible
	if ( file.modelSelectionBoundsVisible )
		thread ShowModelSectionBounds()
}

void function SetupCamZoom( bool calcStartingZoom )
{
	float baseZoom = 120
	vector focusOffset = < 0, 0, 0 >
	switch ( file.currentAssetType )
	{
		case eAssetType.ASSETTYPE_WEAPON:
			focusOffset = WEAPON_FOCUS_OFFSET
			baseZoom = WEAPON_BASE_ZOOM
			break
		case eAssetType.ASSETTYPE_CHARACTER:
			focusOffset = CHARACTER_FOCUS_OFFSET
			baseZoom = CHARACTER_BASE_ZOOM
			break
		case eAssetType.ASSETTYPE_CHARM:
			baseZoom = CHARM_BASE_ZOOM
			break
	}

	spawnNode currentNode	= file.spawnNodes[ file.currentNode ]
	vector forward			= AnglesToForward( currentNode.ang )
	file.zoomTrackEndPos	= OffsetPointRelativeToVector( currentNode.pos, <0, -MAX_DIST_FROM_FOCUS, 0>, forward )
	file.zoomTrackStartPos	= currentNode.pos + focusOffset
	file.zoomNormVec		= Normalize( file.zoomTrackStartPos - file.zoomTrackEndPos )

	TraceResults traceResult = TraceLine( file.zoomTrackStartPos, file.zoomTrackEndPos, [], TRACE_MASK_SOLID, TRACE_COLLISION_GROUP_NONE )
	float tracedZoomLimit	 = Distance( file.zoomTrackStartPos, traceResult.endPos )
	file.maxZoomAmount = Distance( file.zoomTrackStartPos, file.zoomTrackEndPos )
	file.maxZoomAmount = tracedZoomLimit < file.maxZoomAmount ? tracedZoomLimit : file.maxZoomAmount

	if ( calcStartingZoom )
	{
		float zoomAmount
		if ( file.hasCurrentModelBounds )
		{
			float halfModelBoundsHeight = GetCurrentModelHeight() / 2.0
			float halfModelBoundsWidth = GetCurrentModelWidth() / 2.0

			float horizontalFOV = DegToRad( file.viewerCameraFOV )
			float hwRatio =  file.screenHeight / file.screenWidth

			float topPlanAng = atan( tan( horizontalFOV / 2.0 ) * hwRatio )
			float topPlanToModelBoundsAng = halfPi - topPlanAng
			float zoomDistFromBoundsHeight = ( ( halfModelBoundsHeight * sin( topPlanToModelBoundsAng ) ) / sin( topPlanAng ) )

			float leftPlanAng = ( horizontalFOV / 2.0 )
			float leftPlanToModelBoundsAng = halfPi - leftPlanAng
			float zoomDistFromBoundsWidth = ( ( halfModelBoundsWidth * sin( leftPlanToModelBoundsAng ) ) / sin( leftPlanAng ) )

			float zoomDistFromBounds = zoomDistFromBoundsHeight > zoomDistFromBoundsWidth ? zoomDistFromBoundsHeight : zoomDistFromBoundsWidth
			zoomAmount = zoomDistFromBounds + ( GetCurrentModelDepth() / 2 )
			zoomAmount += zoomDistFromBounds / 10.0 
		}
		else
		{
			zoomAmount = baseZoom
		}

		vector dest = file.zoomTrackStartPos - ( file.zoomNormVec * zoomAmount )
		file.viewerCameraEntity.SetOrigin( dest )
		file.lastZoomVal = zoomAmount
	}
	else
	{
		file.lastZoomVal = min( file.lastZoomVal, file.maxZoomAmount )

		vector dest = file.zoomTrackStartPos - ( file.zoomNormVec * file.lastZoomVal )
		file.viewerCameraEntity.SetOrigin( dest )
	}
}

void function ArtViewer_SetCameraZoomPos()
{
	float newIncrement = 0.0

	if ( IsControllerModeActive() )
	{
		float stickLTriggerRaw         = clamp( InputGetAxis( ANALOG_L_TRIGGER ), -1.0, 1.0 )
		float stickLTriggerRemappedAbs = ( fabs( stickLTriggerRaw ) < STICK_DEADZONE ) ? 0.0 : ( ( fabs( stickLTriggerRaw ) - STICK_DEADZONE ) / ( 1.0 - STICK_DEADZONE ) )
		float stickLTrigger            = EaseIn( stickLTriggerRemappedAbs ) * ( stickLTriggerRaw < 0.0 ? -1.0 : 1.0 )

		float stickRTriggerRaw         = clamp( InputGetAxis( ANALOG_R_TRIGGER ), -1.0, 1.0 )
		float stickRTriggerRemappedAbs = ( fabs( stickRTriggerRaw ) < STICK_DEADZONE ) ? 0.0 : ( ( fabs( stickRTriggerRaw ) - STICK_DEADZONE ) / ( 1.0 - STICK_DEADZONE ) )
		float stickRTrigger            = EaseIn( stickRTriggerRemappedAbs ) * ( stickRTriggerRaw < 0.0 ? -1.0 : 1.0 )

		float normalizedTriggerInput = 0.0
		if ( stickLTrigger > 0.0 )
			normalizedTriggerInput = stickLTrigger
		else if ( stickRTrigger > 0.0 )
			normalizedTriggerInput = -stickRTrigger
		else
			return

		float adjustedMaxIncrement = file.maxZoomIncrement_Controller * ( file.lastZoomVal / file.maxZoomAmount )
		newIncrement = normalizedTriggerInput * adjustedMaxIncrement
	}
	else
	{
		float delta = file.mouseWheelLastValue - file.mouseWheelNewValue
		if ( delta == 0.0 )
			return

		file.mouseWheelNewValue = Clamp( file.mouseWheelNewValue, -100.0, 100.0 )
		file.mouseWheelLastValue = file.mouseWheelNewValue

		float adjustedMaxIncrement = file.maxZoomIncrement_Mouse * ( file.lastZoomVal / file.maxZoomAmount )
		newIncrement = delta * adjustedMaxIncrement
	}

	float newZoomVal = Clamp( file.lastZoomVal + newIncrement, 0.0, file.maxZoomAmount )
	float distChange = fabs( newZoomVal + file.lastZoomVal )
	if ( distChange < 0.001 )
		return

	vector destination = file.zoomTrackStartPos - ( file.zoomNormVec * newZoomVal )
	file.viewerCameraEntity.SetOrigin( destination )
	file.lastZoomVal = newZoomVal
}

void function ArtViewer_SetEntityOrigin( vector origin )
{
	file.modelMoverRealOrigin = origin

	vector baseSkinOffs
	if ( file.showBaseSkinSideBySide )
	{
		baseSkinOffs = file.viewerCameraEntity.GetRightVector() * GetSideBySideModelDistance()
	}

	file.modelMover.SetOrigin( origin - baseSkinOffs )
	file.viewerBaseSkinModelMover.SetOrigin( origin + baseSkinOffs )
}

vector function ArtViewer_GetEntityOrigin()
{
	return file.modelMoverRealOrigin
}

void function ArtViewer_SetEntityAngles( vector angles )
{
	file.modelMover.SetAngles( angles )
	file.viewerBaseSkinModelMover.SetAngles( angles )
}

void function ArtViewer_UpdateEntityAngles()
{
	spawnNode node = file.spawnNodes[ file.currentNode ]
	vector pitchTowardCamera = AnglesCompose( node.ang + < 0, 180, 0>, <file.rotationDelta[1], 0, 0> )
	vector newAng            = AnglesCompose( pitchTowardCamera, <0, file.rotationDelta[0], 0> )

	ArtViewer_SetEntityAngles( newAng )
}

void function ArtViewer_UpdateEntityOrigin()
{
	vector transXY	= AnglesToRight( file.viewerCameraEntity.GetAngles() ) * file.movementDelta[0]
	vector transZ 	= <0,0,-1> * file.movementDelta[1]
	vector trans	= transXY + transZ

	trans *= ( file.lastZoomVal * FrameTime() ) 
	vector newOrigin = ArtViewer_GetEntityOrigin() + trans
	spawnNode currentNode = file.spawnNodes[ file.currentNode ]

	vector lowerBounds = ( currentNode.pos - < 0.0, 0.0, BOUNDING_BOX_HEIGHT / 2 > )
	vector upperBounds = ( currentNode.pos + < 0.0, 0.0, BOUNDING_BOX_HEIGHT / 2 > )

	if ( IsBitFlagSet( file.axisLockedFlags, AXIS_LOCKED_X ) )
	{
		trans = < 0.0, 0.0, trans.z >
	}
	else
	{
		if ( GetDistanceSqrFromLineSegment( lowerBounds, upperBounds, newOrigin ) > BOUNDING_BOX_RADIUS )
			trans = < 0.0, 0.0, trans.z >
	}

	if ( IsBitFlagSet( file.axisLockedFlags, AXIS_LOCKED_Y ) )
	{
		trans.z = 0.0
	}
	else
	{
		vector bottomVec     = Normalize( upperBounds - lowerBounds )
		vector pointToBottom = Normalize( newOrigin - lowerBounds )

		vector topVec     = Normalize( lowerBounds - upperBounds )
		vector pointToTop = Normalize( newOrigin - upperBounds )

		if ( DotProduct( bottomVec, pointToBottom ) < 0 || DotProduct( topVec, pointToTop ) < 0.0  )
			trans.z = 0.0
	}

	newOrigin = ArtViewer_GetEntityOrigin() + trans
	ArtViewer_SetEntityOrigin( newOrigin )
}

float function StepTowardsZero( float vel, float amount )
{
	if ( vel > 0.0 )
		return vel > amount ? (  vel - amount ) : 0.0
	else if ( vel < 0.0 )
		return vel < amount ? ( vel + amount ) : 0.0

	return 0.0
}

void function ArtViewer_UpdateAnglesFromInput()
{
	float[2] currentRotationDelta = file.rotationDelta
	float[2] newRotationDelta
	float maxTurnSpeed = 270

	if ( IsControllerModeActive() )
	{
		float stickXRaw         = clamp( InputGetAxis( ANALOG_RIGHT_X ), -1.0, 1.0 )
		float stickXRemappedAbs = ( fabs( stickXRaw ) < STICK_DEADZONE ) ? 0.0 : ( ( fabs( stickXRaw ) - STICK_DEADZONE ) / ( 1.0 - STICK_DEADZONE ) )
		float stickX            = EaseIn( stickXRemappedAbs ) * (stickXRaw < 0.0 ? -1.0 : 1.0)
		file.rotationVel[0] += stickX * maxTurnSpeed * FrameTime() * STICK_ROTATE_SPEED

		float stickYRaw         = clamp( InputGetAxis( ANALOG_RIGHT_Y ), -1.0, 1.0 )
		float stickYRemappedAbs = ( fabs( stickYRaw ) < STICK_DEADZONE ) ? 0.0 : ( ( fabs( stickYRaw ) - STICK_DEADZONE ) / ( 1.0 - STICK_DEADZONE ) )
		float stickY            = EaseIn( stickYRemappedAbs ) * ( -stickYRaw < 0.0 ? -1.0 : 1.0 )
		file.rotationVel[1] -= stickY * maxTurnSpeed * FrameTime() * STICK_ROTATE_SPEED
	}
	else
	{
		file.rotationVel[0] += file.mouseDelta[0]
		file.mouseDelta[0] = 0 

		file.rotationVel[1] += file.mouseDelta[1]
		file.mouseDelta[1] = 0 
	}

	newRotationDelta[0] = currentRotationDelta[0] + (file.rotationVel[0] * FrameTime()) % 360.0
	newRotationDelta[1] = currentRotationDelta[1] + (file.rotationVel[1] * FrameTime()) % 360.0

	float velDecay = 180 * FrameTime()
	float dampX = max( fabs( file.rotationVel[0] ) / 60.0, 0.1 )
	float dampY = max( fabs( file.rotationVel[1] ) / 60.0, 0.1 )
	file.rotationVel[0] = clamp( StepTowardsZero( file.rotationVel[0], velDecay * dampX ), -maxTurnSpeed, maxTurnSpeed )
	file.rotationVel[1] = clamp( StepTowardsZero( file.rotationVel[1], velDecay * dampY ), -maxTurnSpeed, maxTurnSpeed )

	if ( currentRotationDelta[0] == newRotationDelta[0] && currentRotationDelta[1] == newRotationDelta[1] )
		return

	if ( !IsBitFlagSet( file.axisLockedFlags, AXIS_LOCKED_Y ) )
		currentRotationDelta[0] = newRotationDelta[0]
	if ( !IsBitFlagSet( file.axisLockedFlags, AXIS_LOCKED_X ) )
		currentRotationDelta[1] = newRotationDelta[1]

	ArtViewer_UpdateEntityAngles()
}

void function ArtViewer_UpdateOriginFromInput()
{
	float[2] currentMoveDelta = file.movementDelta
	float[2] newMoveDelta
	float maxMoveSpeed = 27

	if ( IsControllerModeActive() )
	{
		float stickXRaw         = clamp( InputGetAxis( ANALOG_RIGHT_X ), -1.0, 1.0 )
		float stickXRemappedAbs = ( fabs( stickXRaw ) < STICK_DEADZONE ) ? 0.0 : ( ( fabs( stickXRaw ) - STICK_DEADZONE ) / ( 1.0 - STICK_DEADZONE ) )
		float stickX            = EaseIn( stickXRemappedAbs ) * (stickXRaw < 0.0 ? -1.0 : 1.0)
		newMoveDelta[0] = stickX * maxMoveSpeed * FrameTime() * STICK_MOVE_SPEED

		float stickYRaw         = clamp( InputGetAxis( ANALOG_RIGHT_Y ), -1.0, 1.0 )
		float stickYRemappedAbs = ( fabs( stickYRaw ) < STICK_DEADZONE ) ? 0.0 : ( ( fabs( stickYRaw ) - STICK_DEADZONE ) / ( 1.0 - STICK_DEADZONE ) )
		float stickY            = EaseIn( stickYRemappedAbs ) * ( -stickYRaw < 0.0 ? -1.0 : 1.0 )
		newMoveDelta[1] = -stickY * maxMoveSpeed * FrameTime() * STICK_MOVE_SPEED
	}
	else
	{
		newMoveDelta[0] = file.mouseDelta[0] * MOUSE_MOVE_SPEED
		file.mouseDelta[0] = 0

		newMoveDelta[1] = file.mouseDelta[1] * MOUSE_MOVE_SPEED
		file.mouseDelta[1] = 0
	}

	if ( newMoveDelta[0] == 0.0 && newMoveDelta[1] == 0.0 )
		return

	currentMoveDelta[0] = newMoveDelta[0]
	currentMoveDelta[1] = newMoveDelta[1]

	ArtViewer_UpdateEntityOrigin()
}

bool function ShouldMoveModel()
{
	if ( file.viewerModel == null || file.modelMover == null )
		return false

	if ( file.showCharacterSelectMenu )
		return false

	return true
}

void function MoveModel( entity player )
{
	player.EndSignal( "Stop_MoveModel" )

	while ( ShouldMoveModel() )
	{
		ArtViewer_SetCameraZoomPos()
		if ( file.tumbleModeActive )
			ArtViewer_UpdateAnglesFromInput()
		else
			ArtViewer_UpdateOriginFromInput()

		WaitFrame()
	}

	file.viewerModel.ClearParent()
	file.viewerBaseSkinModel.ClearParent()
}

void function ResetViewerModelOriginAndAngles()
{
	if ( file.viewerModel != null && file.modelMover != null )
	{
		spawnNode node = file.spawnNodes[ file.currentNode ]

		file.previousMousePos[0] = 0
		file.previousMousePos[1] = 0
		file.mouseDelta[0] = 0
		file.mouseDelta[1] = 0

		file.rotationDelta[0] = 0
		file.rotationDelta[1] = 0
		file.rotationVel[0] = 0
		file.rotationVel[1] = 0

		file.movementDelta[0] = 0
		file.movementDelta[1] = 0

		ArtViewer_SetEntityOrigin( node.pos )
		ArtViewer_SetEntityAngles( node.ang + <0, 180, 0> ) 
	}
}

void function SetCharmModel()
{
	file.viewerModel.Hide()

	Assert( file.availableAssets.len() > 0 )
	if ( file.availableAssets.len() == 0 )
	{
		Warning( "Art Viewer - ERROR: No avaliable charms to update with." )

		if ( IsValid( file.viewerCharmModel ) )
			file.viewerCharmModel.Hide()

		return
	}

	ItemFlavor charmToApply = file.availableAssets[ file.currentAsset ]
	string charmModel = WeaponCharm_GetCharmModel( charmToApply )
	if ( charmModel == "" )
	{
		if ( IsValid( file.viewerCharmModel ) )
			file.viewerCharmModel.Hide()

		return
	}

	if ( IsValid( file.viewerCharmModel ) )
		file.viewerCharmModel.Destroy()

	file.viewerCharmModel = CreateClientSidePropDynamicCharm( <0, 0, 0>, CHARM_ROTATION_OFFSET, charmModel )

	Assert( IsValid( file.modelMover ) )
	if ( !IsValid( file.modelMover ) )
	{
		file.viewerCharmModel.Hide()
		return
	}

	file.viewerCharmModel.SetOrigin( ArtViewer_GetEntityOrigin() + <0, 0, 5.0> )
	file.viewerCharmModel.SetParent( file.modelMover, "", true )
	file.viewerCharmModel.SetModelScale( VIEWER_CHARM_SIZE )
}

void function UpdateCharacterSkin()
{
	file.viewerModel.SetAngles( <0, 0, 0> )
	file.viewerBaseSkinModel.SetAngles( <0, 0, 0> )

	Assert( file.availableAssetSkins.len() > 0 )
	if ( file.availableAssetSkins.len() == 0 )
	{
		Warning( "Art Viewer - ERROR: No avaliable character skins to update with." )
		return
	}

	file.viewerModel.Show()
	if ( IsValid( file.viewerCharmModel ) )
		file.viewerCharmModel.Hide()

	ItemFlavor skinToApply = file.availableAssetSkins[ file.currentAssetSkin ]
	CharacterSkin_Apply( file.viewerModel, skinToApply )

	SetNestedGladiatorCardOverrideCharacter( file.nestedGCHandle, file.availableAssets[ file.currentAsset ] )
	SetNestedGladiatorCardOverrideSkin( file.nestedGCHandle, skinToApply )

	
	LoadoutEntry defaultLoadoutSkin = Loadout_CharacterSkin( file.availableAssets[ file.currentAsset ] )
	ItemFlavor defaultFlavor = GetDefaultItemFlavorForLoadoutSlot( EHI_null, defaultLoadoutSkin )
	file.viewerBaseSkinModel.SetModel( CharacterSkin_GetBodyModel( defaultFlavor ) )

	file.modelMover.SetOrigin( GetViewerModelRotatePoint() )
	file.viewerBaseSkinModelMover.SetOrigin( GetViewerModelRotatePoint() )

	if ( file.showCharacterSelectMenu )
	{
		UpdateCharacterSelectLightingAndBackground( skinToApply )
	}
}

void function UpdateWeaponSkin()
{
	Assert( file.availableAssetSkins.len() > 0 )
	if ( file.availableAssetSkins.len() == 0 )
	{
		Warning( "Art Viewer - ERROR: No avaliable weapon skins to update with." )
		return
	}

	ItemFlavor skinToApply = file.availableAssetSkins[ file.currentAssetSkin ]
	ItemFlavor baseSkin = GetDefaultItemFlavorForLoadoutSlot( EHI_null, Loadout_WeaponSkin( file.availableAssets[ file.currentAsset ] ) )

	file.viewerModel.Show()
	if ( IsValid( file.viewerCharmModel ) )
		file.viewerCharmModel.Hide()

	if ( file.useWorldModel )
	{
		DestroyCharmForWeaponEntity( file.viewerModel )

		asset model = WeaponSkin_GetWorldModel( skinToApply )
		file.viewerModel.SetModel( model )

		int skinIndex = file.viewerModel.GetSkinIndexByName( WeaponSkin_GetSkinName( skinToApply ) )
		int camoIndex = WeaponSkin_GetCamoIndex( skinToApply )

		if ( skinIndex == -1 )
		{
			skinIndex = 0
			camoIndex = 0
		}

		if ( camoIndex >= CAMO_SKIN_COUNT )
		{
			Assert ( false, "Tried to set camoIndex of " + string(camoIndex) + " but the maximum index is " + string(CAMO_SKIN_COUNT) )
			camoIndex = 0
		}

		file.viewerModel.SetSkin( skinIndex )
		file.viewerModel.SetCamo( camoIndex )

		file.viewerBaseSkinModel.SetModel( WeaponSkin_GetWorldModel( baseSkin ) )
	}
	else
	{
		ItemFlavor ornull charmToApply = null
		if ( file.availableCharms.len() > 0 )
			charmToApply = file.availableCharms[ file.currentCharm ]

		WeaponCosmetics_Apply( file.viewerModel, skinToApply, charmToApply )
		file.viewerCharmModel = GetCharmForWeaponEntity( file.viewerModel )
		file.viewerBaseSkinModel.SetModel( WeaponSkin_GetViewModel( baseSkin ) )
	}

	bool isReactive = WeaponSkin_DoesReactToKills( skinToApply )
	ItemFlavor weaponFlavor = WeaponSkin_GetWeaponFlavor( skinToApply )
	if ( isReactive )
		MenuWeaponModel_ApplyReactiveSkinBodyGroup( skinToApply, weaponFlavor, file.viewerModel )
	else
		ShowDefaultBodygroupsOnFakeWeapon( file.viewerModel, WeaponItemFlavor_GetClassname( weaponFlavor ) )

	MenuWeaponModel_ClearReactiveEffects( file.viewerModel )
	if ( isReactive )
	{
		MenuWeaponModel_StartReactiveEffects( file.viewerModel, skinToApply )
	}

	file.viewerModel.kv.rendercolor = "0 0 0 255" 
	file.viewerModel.SetAngles( WEAPON_ROTATION_OFFSET ) 

	int MenuAttachmentID = file.viewerModel.LookupAttachment( "MENU_ROTATE" )
	if ( MenuAttachmentID != 0 )
	{
		file.modelMover.SetOrigin( file.viewerModel.GetAttachmentOrigin( MenuAttachmentID ) )
	}
	else
	{
		if ( !file.useWorldModel )
			Warning( "Failed to find weapon rotate attachment. Falling back onto the model rotate point." )

		file.modelMover.SetOrigin( GetViewerModelRotatePoint() )
	}


	file.viewerBaseSkinModel.SetAngles( WEAPON_ROTATION_OFFSET )

	int baseMenuAttachmentID = file.viewerBaseSkinModel.LookupAttachment( "MENU_ROTATE" )
	if ( baseMenuAttachmentID != 0 )
	{
		file.viewerBaseSkinModelMover.SetOrigin( file.viewerModel.GetAttachmentOrigin( baseMenuAttachmentID ) )
	}
	else
	{
		if ( !file.useWorldModel )
			Warning( "Failed to find weapon rotate attachment for base skin. Falling back onto the model rotate point." )

		file.viewerBaseSkinModelMover.SetOrigin( GetViewerModelRotatePoint() )
	}
}

float function GetSideBySideModelDistance()
{
	return Distance2D( file.modelBounds.mins, file.modelBounds.maxs ) / SIDEBYSIDE_DIST_SCALE
}

void function UpdateBaseSkinModel()
{
	asset baseModel = EMPTY_MODEL

	if ( file.showBaseSkin )
		file.viewerBaseSkinModel.Show()
	else
		file.viewerBaseSkinModel.Hide()

	
	ArtViewer_SetEntityOrigin( file.modelMoverRealOrigin )
}

void function UpdateCurrentWeaponClassname()
{
	OVSetCurrentWeaponClassname( GetGlobalSettingsString( ItemFlavor_GetAsset( file.availableAssets[ file.currentAsset ] ), "entityClassname" ) )
}

void function UpdateModelViewerSkin()
{
	entity player = GetLocalClientPlayer()
	player.Signal( "Stop_MoveModel" )

	Assert( IsValid( file.modelMover ) && IsValid( file.viewerModel ) )
	if ( !IsValid( file.modelMover ) || !IsValid( file.viewerModel ) )
		return

	bool oldSideBySide = file.showBaseSkinSideBySide
	file.showBaseSkinSideBySide = false
	ArtViewer_SetEntityOrigin( file.modelMoverRealOrigin )

	vector prevAng = file.modelMover.GetAngles()
	vector prevPos = ArtViewer_GetEntityOrigin()

	ArtViewer_SetEntityAngles( <0, 0, 0> )
	file.viewerModel.ClearParent()
	file.viewerBaseSkinModel.ClearParent()

	switch ( file.currentAssetType )
	{
		case eAssetType.ASSETTYPE_CHARACTER:
			UpdateCharacterSkin()
			break
		case eAssetType.ASSETTYPE_WEAPON:
			UpdateWeaponSkin()
			break
		case eAssetType.ASSETTYPE_CHARM:
			SetCharmModel()
			break
		default:
			Warning( "Art Viewer - ERROR: Unexpected asset type: ", file.currentAssetType )
			return
	}

	file.viewerModel.SetParent( file.modelMover, "", true )
	file.viewerBaseSkinModel.SetParent( file.viewerBaseSkinModelMover, "", true )
	ArtViewer_SetEntityAngles( prevAng )
	ArtViewer_SetEntityOrigin( prevPos )

	file.showBaseSkinSideBySide = oldSideBySide
	UpdateBaseSkinModel()

	OVPostModelUpdate()

	SetupCamZoom( file.shouldResetCameraOnModelUpdate )
	if ( file.shouldResetCameraOnModelUpdate )
		file.shouldResetCameraOnModelUpdate = false

	thread MoveModel( player )
}

void function UpdateModelToCurrentEnvNode()
{
	Assert( IsValid( file.modelMover ) )

	spawnNode node = file.spawnNodes[ file.currentNode ]
	if ( IsValid( file.modelMover ) )
	{
		UpdateSunAngles( node.ang )

		file.viewerCameraEntity.SetAngles( node.ang )
		SetupCamZoom( false )

		spawnNode prevNode = file.spawnNodes[ file.previousNode ]
		float dist = Distance( prevNode.pos, ArtViewer_GetEntityOrigin() )
		vector normVec = Normalize( ArtViewer_GetEntityOrigin() - prevNode.pos  )

		float angDiff = ( node.ang.y - prevNode.ang.y )
		vector posOffset = ( RotateVector( normVec, < 0, angDiff, 0 > ) * dist )
		ArtViewer_SetEntityOrigin( node.pos + posOffset )
		ArtViewer_SetEntityAngles( file.modelMover.GetAngles() + < 0, angDiff, 0 > )
	}
	else
		Warning( "Art Viewer - ERROR: Invalid model mover on current node update." )
}

void function UpdateSunAngles( vector angles )
{
	entity lightEnvironmentEntity = GetLightEnvironmentEntity()

	Assert( IsValid( lightEnvironmentEntity ) )
	if ( !IsValid( lightEnvironmentEntity ) )
	{
		Warning( "Art Viewer - ERROR: Invalid Light Environment Entity on sun angle update." )
		return
	}

	angles += file.shouldBeInShadow ? < 0, 180, 0 > :  < 0, 0, 0 >
	lightEnvironmentEntity.OverrideAngles( SUN_PITCH, angles.y )
}

vector function GetViewerModelRotatePoint()
{
	if ( !file.hasCurrentModelBounds || file.showCharacterSelectMenu )
	{
		return file.viewerModel.GetOrigin()
	}

	vector mins = file.modelBounds.mins
	vector maxs = file.modelBounds.maxs

	AABB rotatedBounds = RotateAABB( mins, maxs, file.viewerModel.GetAngles() )

	mins = rotatedBounds.mins
	maxs = rotatedBounds.maxs

	return file.viewerModel.GetOrigin() + <mins.x + ( ( maxs.x - mins.x ) * 0.5 ), mins.y + ( ( maxs.y - mins.y ) * 0.5 ), mins.z + ( ( maxs.z - mins.z ) * 0.5 )>
}

float function GetCurrentModelWidth()
{
	return file.modelBounds.maxs.x - file.modelBounds.mins.x
}

float function GetCurrentModelHeight()
{
	return file.modelBounds.maxs.z - file.modelBounds.mins.z
}

float function GetCurrentModelDepth()
{
	return file.modelBounds.maxs.y - file.modelBounds.mins.y
}

void function OutsourceViewer_ResetView()
{
	spawnNode currNode = file.spawnNodes[ file.currentNode ]
	file.viewerCameraEntity.SetAngles( currNode.ang )

	SetupCamZoom( true )
}

void function InitArtViewerEnvironments()
{
	array<string> spawnNodeNames
	array<entity> infoTargetEntities = GetClientEntArrayBySignifier( OUTSOURCE_INFO_TARGET_CLASS_NAME )
	if ( infoTargetEntities.len() > 0 )
	{
		foreach ( infoTarget in infoTargetEntities )  
		{
			string infoTargetName = infoTarget.GetScriptName()
			if ( infoTargetName.find( OUTSOURCE_VIEWER_NODES_SCRIPT_NAME ) >= 0 )
			{
				spawnNode newNode
				newNode.pos = infoTarget.GetOrigin()
				newNode.ang = infoTarget.GetAngles()
				file.spawnNodes.append( newNode )

				
				string envName = infoTargetName.slice( OUTSOURCE_VIEWER_NODES_SCRIPT_NAME.len() )
				envName = strip( envName )
				if ( envName == "" )
				{
					spawnNodeNames.append( "Environment " + file.spawnNodes.len() )
					printt( "Art Viewer - Environment found: ", file.spawnNodes.len() )
				}
				else
				{
					spawnNodeNames.append( envName )
					printt( "Art Viewer - Environment found: ", envName )
				}
			}
		}
	}

	if ( file.spawnNodes.len() == 0 )
	{
		Warning( "Art Viewer - Error: No environments found." )
		return
	}

	printf( "Art Viewer - Environment init complete, found %i environments.", file.spawnNodes.len() )
	InitOutsourceUI_EnvironmentNodes( spawnNodeNames )
}

void function OVSpawnModels( vector pos, vector ang )
{
	file.currentAssetType	= eAssetType.ASSETTYPE_CHARACTER
	file.currentAsset		= 0
	file.currentAssetSkin	= 0
	file.currentCharm		= 0

	file.modelMover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", pos, ang + <0, 180, 0> ) 
	file.viewerBaseSkinModelMover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", pos, ang + <0, 180, 0> )
	file.viewerModel = CreateOVClientSideEntity( pos, ang + <0, 180, 0>, $"mdl/dev/empty_model.rmdl" )
	file.viewerBaseSkinModel = CreateClientSidePropDynamic( pos, ang + <0, 180, 0>, $"mdl/dev/empty_model.rmdl" )
	ArtViewer_SetEntityOrigin( pos )

	if ( !IsValid( file.modelMover ) || !IsValid( file.viewerModel ) || !IsValid( file.viewerBaseSkinModel ) )
	{
		Warning( "Art Viewer - Error: Failed on model spawning, shutting down." )
		OVShutdown()
		return
	}

	file.viewerModel.SetFadeDistance( MODEL_FADE_DIST )
	file.viewerCameraFOV = DEFAULT_FOV
	file.viewerCameraEntity = CreateClientSidePointCamera( <0, 0, 0>, <0, 0, 0>, file.viewerCameraFOV )
	file.cubemapSpheres = CreateClientSidePropDynamic( < 0, 0, 0 >, < 0, 0, 0 >, $"mdl/dev/envballs.rmdl" )
	file.cubemapSpheres.SetParent( file.viewerCameraEntity )
	file.cubemapSpheres.Hide()
	GetLocalClientPlayer().SetMenuCameraEntityWithAudio( file.viewerCameraEntity )

	RefreshAssetItemFlavors()
	file.shouldResetCameraOnModelUpdate = true
	ClientCodeCallback_UpdateOutsourceModel( ARTVIEWER_PROPERTIES_ASSETTYPE, 0 )

	OutsourceViewer_ResetView()
	UpdateSunAngles( ang )
}

void function OVStart()
{
	if ( file.outsourceViewerRunning )
		return

	InitArtViewerEnvironments()
	if ( file.spawnNodes.len() == 0 ) 
		return

	file.outsourceViewerRunning = true

	file.viewerModel = null
	file.modelMover = null

	InitCharacterSelectMenu()
	EnableViewerWatermark()
	InitViewerGladiatorCard()
	UpdateMainHudVisibility( GetLocalViewPlayer() )

	RegisterSignal( "Stop_MoveModel" )

	file.screenHeight = float( GetScreenSize().height )
	file.screenWidth = float( GetScreenSize().width )

	OVSpawnModels( file.spawnNodes[0].pos, file.spawnNodes[0].ang )
}

void function OVShutdown()
{
	if ( !file.outsourceViewerRunning )
		return
	file.outsourceViewerRunning = false

	file.spawnNodes.clear()
	file.currentNode = 0
	file.previousNode = 0

	if ( IsValid( file.viewerModel ) )
		file.viewerModel.Destroy()

	if ( IsValid( file.modelMover ) )
		file.modelMover.Destroy()

	if ( IsValid( file.viewerBaseSkinModel ) )
		file.viewerBaseSkinModel.Destroy()

	if ( IsValid( file.viewerBaseSkinModelMover ) )
		file.viewerBaseSkinModelMover.Destroy()

	if ( IsValid( file.cubemapSpheres ) )
		file.cubemapSpheres.Destroy()

	DisableViewerWatermark()
	DestroyCharacterSelectMenu()
	DestroyViewerGladiatorCard()

	entity player = GetLocalClientPlayer()
	player.ClearMenuCameraEntity()
	UpdateMainHudVisibility( player )

	if ( IsValid( file.viewerCameraEntity ) )
		file.viewerCameraEntity.Destroy()
}
#endif





void function ServerCallback_OVToggle()
{
#if DEV
	if ( !file.outsourceViewerRunning )
	{
		OVStart()
	}
	else
	{
		OVShutdown()
	}
#endif
}

void function ServerCallback_OVUpdateModelBounds( vector min, vector max )
{
#if DEV
	file.hasCurrentModelBounds = true
	table<string, vector> tab = { mins = min, maxs = max }
	file.modelBounds = tab
	UpdateModelViewerSkin()
#endif
}






void function ClientCodeCallback_ResetCamera()
{
#if DEV
	OutsourceViewer_ResetView()
#endif
}

void function ClientCodeCallback_ResetModel()
{
#if DEV
	ResetViewerModelOriginAndAngles()
#endif
}

void function ClientCodeCallback_SwitchOutsourceEnv( int envNum )
{
#if DEV
	if ( envNum < 0 || envNum >= file.spawnNodes.len() )
	{
		Warning( "Art Viewer - Error: Tried to switch to an invalid node" )
		return
	}

	file.previousNode = file.currentNode
	file.currentNode = envNum

	
	
	
	if ( !file.showCharacterSelectMenu )
		UpdateModelToCurrentEnvNode()
#endif
}

void function ClientCodeCallback_UpdateOutsourceModel( int pickerProperty, int updatedValue )
{
#if DEV
	switch ( pickerProperty )
	{
		case ARTVIEWER_PROPERTIES_ASSETTYPE:
			if ( updatedValue >= eAssetType._count )
				return

			file.currentAssetType = updatedValue
			file.currentAsset = 0
			file.currentAssetSkin = 0
			file.shouldResetCameraOnModelUpdate = true

			if ( file.currentAssetType != eAssetType.ASSETTYPE_CHARM && file.currentAssetType != eAssetType.ASSETTYPE_WEAPON )
				file.currentCharm = 0

			RefreshAssetItemFlavors()
			break
		case ARTVIEWER_PROPERTIES_ASSET:
			if ( updatedValue >= file.availableAssets.len() )
				return

			if ( file.currentAssetType == eAssetType.ASSETTYPE_CHARM )
				file.currentCharm = updatedValue
			else
				file.currentAsset = updatedValue

			file.currentAssetSkin = 0
			RefreshAssetItemFlavors()
			break
		case ARTVIEWER_PROPERTIES_SKIN:
			if ( updatedValue >= file.availableAssetSkins.len() )
				return

			file.currentAssetSkin = updatedValue
			break
		case ARTVIEWER_PROPERTIES_CHARM:
			if ( updatedValue >= file.availableCharms.len() )
				return

			file.currentCharm = updatedValue
			break
		case ARTVIEWER_PROPERTIES_WORLDMODEL:
			file.useWorldModel = bool( updatedValue )
			break
		default:
			return
			break
	}

	file.hasCurrentModelBounds = false
	string modelName
	ItemFlavor currentSkin
	switch ( file.currentAssetType )
	{
		case eAssetType.ASSETTYPE_CHARACTER:
			if ( file.availableAssetSkins.len() == 0  )
				return

			currentSkin = file.availableAssetSkins[ file.currentAssetSkin ]
			modelName = string( CharacterSkin_GetBodyModel( currentSkin ) )
			break
		case eAssetType.ASSETTYPE_WEAPON:
			if ( file.availableAssetSkins.len() == 0  )
				return

			currentSkin = file.availableAssetSkins[ file.currentAssetSkin ]
			modelName = string( WeaponSkin_GetWorldModel( currentSkin ) )
			break
		case eAssetType.ASSETTYPE_CHARM:
			UpdateModelViewerSkin()
			return
			break
		default:
			return
			break
	}

	if ( modelName != "" )
	{
		Remote_ServerCallFunction( "ClientCallback_GetModelBounds", file.currentAssetType, currentSkin.guid )
	}
#endif
}

void function ClientCodeCallback_SetAxisLockedFlags( int flags )
{
#if DEV
	file.axisLockedFlags = flags
#endif
}

void function ClientCodeCallback_UpdateMousePos( float posX, float posY, bool imGuiFocused )
{
#if DEV
	if ( IsControllerModeActive() )
		return

	float screenScaleXModifier = 1920.0 / file.screenWidth 
	float mousePosXAdjustedForScale  = posX * screenScaleXModifier

	float screenScaleYModifier = 1080.0 / file.screenHeight 
	float mousePosYAdjustedForScale  = posY * screenScaleYModifier

	if ( posX >= 0.0 && posY >= 0.0 && posX <= file.screenWidth && posY <= file.screenHeight )
	{
		if ( InputIsButtonDown( MOUSE_LEFT ) && !imGuiFocused )
		{
			file.mouseDelta[0] = mousePosXAdjustedForScale - file.previousMousePos[0]
			file.mouseDelta[1] = mousePosYAdjustedForScale - file.previousMousePos[1]
		}
	}

	file.previousMousePos[0] = mousePosXAdjustedForScale
	file.previousMousePos[1] = mousePosYAdjustedForScale
#endif
}

void function ClientCodeCallback_InputMouseScrolledUp()
{
#if DEV
	file.mouseWheelNewValue++
#endif
}

void function ClientCodeCallback_InputMouseScrolledDown()
{
#if DEV
	file.mouseWheelNewValue--
#endif
}

void function ClientCodeCallback_SetMoveModel( bool moveModel )
{
#if DEV
	file.tumbleModeActive = !moveModel

	file.rotationVel[0] = 0
	file.rotationVel[1] = 0
#endif
}
void function ClientCodeCallback_ToggleModelInShadow( bool inShadow )
{
#if DEV
	file.shouldBeInShadow = inShadow
	UpdateSunAngles( file.spawnNodes[ file.currentNode ].ang )
#endif
}

void function ClientCodeCallback_UpdateOutsourceBaseModel( bool enableBaseModel, bool sideBySide )
{
#if DEV
	if ( file.showCharacterSelectMenu )
	{
		characterSelectMenu.restoreShowBaseSkin = enableBaseModel
		characterSelectMenu.restoreShowBaseSkinSideBySide = sideBySide
	}
	else
	{
		
		file.showBaseSkin = enableBaseModel
		file.showBaseSkinSideBySide = sideBySide

		
		if ( file.currentAssetType > eAssetType.ASSETTYPE_WEAPON )
		{
			Warning( "Code tried to call ClientCodeCallback_ToggleOutsourceBaseModel while invalid asset type was selected!" )
			return
		}

		UpdateBaseSkinModel()
	}
#endif
}

void function ClientCodeCallback_ToggleOutsourceCubemapSpheres( bool enableCubemapSpheres )
{
#if DEV
	if ( enableCubemapSpheres )
	{
		file.cubemapSpheres.Show()
	}
	else
	{
		file.cubemapSpheres.Hide()
	}
#endif
}

void function ClientCodeCallback_ToggleCharacterSelectMenu( bool enableCharacterSelectMenu )
{
#if DEV
		entity player = GetLocalClientPlayer()
		if ( enableCharacterSelectMenu )
		{
			if ( file.currentAssetType != eAssetType.ASSETTYPE_CHARACTER || file.showCharacterSelectMenu )
				return

			player.SetMenuCameraEntityWithAudio( characterSelectMenu.camera )
			file.showCharacterSelectMenu = true

			characterSelectMenu.restoreShowBaseSkin = file.showBaseSkin
			characterSelectMenu.restoreShowBaseSkinSideBySide = file.showBaseSkinSideBySide
			characterSelectMenu.restoreViewModelPos = ArtViewer_GetEntityOrigin()
			characterSelectMenu.restoreViewModelAng = file.modelMover.GetAngles()
			characterSelectMenu.restoreViewModelNode = file.currentNode

			file.showBaseSkin = false
			file.showBaseSkinSideBySide = false

			ArtViewer_SetEntityOrigin( characterSelectMenu.modelNode.GetOrigin() )
			ArtViewer_SetEntityAngles( characterSelectMenu.modelNode.GetAngles() )
			UpdateModelViewerSkin()

			UpdateCharacterSelectLightingAndBackground( file.availableAssetSkins[ file.currentAssetSkin ] )
			RuiSetVisible( characterSelectMenu.vignetteRui, true )
		}
		else
		{
			if ( !file.showCharacterSelectMenu )
				return

			file.showBaseSkin = characterSelectMenu.restoreShowBaseSkin
			file.showBaseSkinSideBySide = characterSelectMenu.restoreShowBaseSkinSideBySide
			ArtViewer_SetEntityOrigin( characterSelectMenu.restoreViewModelPos )
			ArtViewer_SetEntityAngles( characterSelectMenu.restoreViewModelAng )
			file.previousNode = characterSelectMenu.restoreViewModelNode

			file.showCharacterSelectMenu = false
			UpdateModelViewerSkin()

			UpdateModelToCurrentEnvNode()

			player.SetMenuCameraEntityWithAudio( file.viewerCameraEntity )
			RuiSetVisible( characterSelectMenu.vignetteRui, false )
		}
#endif
}

void function ClientCodeCallback_ToggleOutsourceGladCard( bool showGladCard )
{
#if DEV
	RuiSetVisible( file.gladCardRui, showGladCard )
#endif
}
