
global function ShPassPanel_LevelInit



global function UIToClient_StartTempBattlePassPresentationBackground
global function UIToClient_StopTempBattlePassPresentationBackground
global function UIToClient_StopBattlePassScene
global function UIToClient_ItemPresentation
global function TempBattlePassPresentationBackground_Thread
global function InitBattlePassLights
global function BattlePassLightsOn
global function BattlePassLightsOff
global function ClearBattlePassItem
global function MoveLightsToSceneRef

#if DEV
global function DEV_TweakMover
#endif









































global function Season_GetLongName
global function Season_GetShortName
global function Season_GetTimeRemainingText
global function Season_GetSmallLogo
global function Season_GetSmallLogoBg
global function Season_GetLobbyBannerLeftImage
global function Season_GetLobbyBannerRightImage

global function Season_GetTitleTextColor
global function Season_GetHeaderTextColor
global function Season_GetTimeRemainingTextColor
global function Season_GetNewColor
global function Season_GetColor

global function Season_GetTabBarFocusedCol
global function Season_GetTabBarSelectedCol
global function Season_GetTabBGFocusedCol
global function Season_GetTabBGSelectedCol
global function Season_GetTabTextDefaultCol
global function Season_GetTabTextFocusedCol
global function Season_GetTabTextSelectedCol
global function Season_GetTabGlowFocusedCol

global function Season_GetSubTabBarFocusedCol
global function Season_GetSubTabBarSelectedCol
global function Season_GetSubTabBGFocusedCol
global function Season_GetSubTabBGSelectedCol
global function Season_GetSubTabTextDefaultCol
global function Season_GetSubTabTextFocusedCol
global function Season_GetSubTabTextSelectedCol
global function Season_GetSubTabGlowFocusedCol







struct BattlePassPageData
{
	int startLevel
	int endLevel
}


const float BATTLEPASS_MODEL_ROTATE_SPEED = 15.0

global const string FEATURE_EVENT_BOOSTED_TUTORIAL = "eventboosted"

struct FileStruct_LifetimeLevel
{

		bool                         isTempBattlePassPresentationBackgroundThreadActive = false
		float                        rotateSpeed = BATTLEPASS_MODEL_ROTATE_SPEED
		string						 sceneRefName
		vector                       sceneRefOrigin
		vector                       sceneRefAngles
		entity                       mover
		array<entity>                models
		array<int>                	 fxs
		NestedGladiatorCardHandle&   bannerHandle
		var                          topo
		var                          rui
		array<entity>                stationaryLights
		table<entity, vector>        stationaryLightOffsets
		
		
		string                       playingPreviewAlias

		var loadscreenPreviewBox = null







	table signalDummy
	int   videoChannel = -1
}
FileStruct_LifetimeLevel& fileLevel

























struct
{





















































} file












void function ShPassPanel_LevelInit()
{

		RegisterSignal( "StopTempBattlePassPresentationBackgroundThread" )
		RegisterButtonPressedCallback( MOUSE_WHEEL_UP, OnMouseWheelUp )
		RegisterButtonPressedCallback( MOUSE_WHEEL_DOWN, OnMouseWheelDown )

		AddCallback_UIScriptReset( void function() {
			fileLevel.loadscreenPreviewBox = null 
		} )










}



























































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































void function UIToClient_StartTempBattlePassPresentationBackground( asset bgImage )
{
	
	if ( fileLevel.isTempBattlePassPresentationBackgroundThreadActive )
		return
	thread TempBattlePassPresentationBackground_Thread( bgImage )
}




void function UIToClient_StopTempBattlePassPresentationBackground()
{
	
	Signal( fileLevel.signalDummy, "StopTempBattlePassPresentationBackgroundThread" )
	
}




void function UIToClient_StopBattlePassScene()
{
	ClearBattlePassItem()
}










struct CarouselColumnState
{
	int    level = -1
	var    topo
	var    rui
	var    columnClickZonePanel
	entity reward1Model
	var    reward1DetailsPanel
	entity reward2Model
	var    reward2DetailsPanel
	entity light
	float  growSize = 0.0
}


void function TempBattlePassPresentationBackground_Thread( asset bgImage )
{
	Signal( fileLevel.signalDummy, "StopTempBattlePassPresentationBackgroundThread" ) 
	EndSignal( fileLevel.signalDummy, "StopTempBattlePassPresentationBackgroundThread" )

	fileLevel.isTempBattlePassPresentationBackgroundThreadActive = true

	entity cam = clGlobal.menuCamera
	
	
	

	float camSceneDist = 100.0
	vector camOrg      = cam.GetOrigin()
	vector camAng      = cam.GetAngles()
	vector camForward  = AnglesToForward( camAng )
	vector camRight    = AnglesToRight( camAng )
	vector camUp       = AnglesToUp( camAng )

	float lol          = 0.2
	vector bgCenterPos = camOrg + 600.0 * camForward - lol * 1920.0 * 0.5 * camRight + lol * 1080.0 * 0.5 * camUp
	var bgTopo         = RuiTopology_CreatePlane( bgCenterPos, lol * 1920.0 * camRight, lol * 1080.0 * -camUp, false )
	
	var bgRui          = RuiCreate( $"ui/lobby_battlepass_temp_bg.rpak", bgTopo, RUI_DRAW_WORLD, 10000 )
	
	RuiSetImage( bgRui, "bgImage", bgImage )
	

	entity bgModel = CreateClientSidePropDynamic( bgCenterPos + 50.0 * camForward, -camForward, $"mdl/menu/loot_ceremony_stat_tracker_bg.rmdl" )
	bgModel.MakeSafeForUIScriptHack()
	bgModel.SetModelScale( 20.0 )

	OnThreadEnd( function() : ( bgTopo, bgRui, bgModel ) {
		fileLevel.isTempBattlePassPresentationBackgroundThreadActive = false

		RuiDestroy( bgRui )
		RuiTopology_Destroy( bgTopo )
		if ( IsValid( bgModel ) )
			bgModel.Destroy()
	} )

	WaitForever()
}




void function OnMouseWheelUp( entity unused )
{
	
}




void function OnMouseWheelDown( entity unused )
{
	
}















































































































































































































































































































































































































































































































































void function UIToClient_ItemPresentation( SettingsAssetGUID itemFlavorGUID, int level, float scale, bool showLow, var loadscreenPreviewBox, bool shouldPlayAudioPreview, string sceneRefName, bool isNXHH = false, bool isMilestoneEvent = false, bool isBattlepassMilestone = false, bool useMenuZoomOffset = true, vector offset = <0, 0, 0>  )
{
	ItemFlavor flav = GetItemFlavorByGUID( itemFlavorGUID )
	int itemType = ItemFlavor_GetType( flav )
	entity sceneRef = GetEntByScriptName( sceneRefName )

	fileLevel.sceneRefName = sceneRefName
	fileLevel.sceneRefOrigin = sceneRef.GetOrigin()

	bool showEmoteBase = true

	if ( sceneRefName == "battlepass_right_ref" )
	{
		if ( fabs( float( GetScreenSize().width ) / float( GetScreenSize().height ) - (16.0 / 10.0) ) < 0.07 )
			fileLevel.sceneRefOrigin += <0, 25, 0>

		if ( itemType == eItemType.character_emote )
		{
			scale *= 0.7
		}
#if !PC_PROG_NX_UI
		else if ( itemType == eItemType.emote_icon )
		{
			if ( GetNearestAspectRatio( GetScreenSize().width, GetScreenSize().height ) == 1.6 )
			{
				fileLevel.sceneRefOrigin += <10, 60, 0>
			}
			else
			{
				fileLevel.sceneRefOrigin += <12, 60, 0>
			}
		}
#endif
	}
	else if ( sceneRefName == "battlepass_center_ref" )
	{
		
		
		if ( itemType == eItemType.emote_icon )
		{

				fileLevel.sceneRefOrigin += <5, 50, 21>



		}
		else if ( itemType == eItemType.character_emote )
		{
			scale *= 0.7
		}
		else if ( itemType == eItemType.artifact_component_deathbox || itemType == eItemType.artifact_component_activation_emote )
		{
			fileLevel.sceneRefOrigin += <0, 0, 10>
			scale *= 1.8
		}

		else if ( itemType == eItemType.character_skin || itemType == eItemType.character )
		{
			fileLevel.sceneRefOrigin += <0, 0, -10>
			scale *= 1.25
		}
		else if ( itemType == eItemType.weapon_skin )
		{
			asset video = WeaponSkin_GetVideo( flav )
			if ( video == $"" )
			{
				fileLevel.sceneRefOrigin += <0, 0, -2>
				scale *= 2.0 
				ItemFlavor ornull weaponFlav = GetItemFlavorAssociatedCharacterOrWeapon( flav )
				if ( weaponFlav != null )
				{
					expect ItemFlavor( weaponFlav )
					if ( ItemFlavor_GetType( weaponFlav ) == eItemType.loot_main_weapon )
					{
						asset weaponCategory = GetGlobalSettingsAsset( ItemFlavor_GetAsset( weaponFlav ), "category" )
						if ( weaponCategory == "settings/itemflav/weapon_category/pistol.rpak" )
						{
							scale *= 0.75 
						}
					}
				}
			}
			else
			{
				scale *= 1.5 
			}
		}
		else if ( itemType == eItemType.skydive_emote )
		{
			scale *= 1.5 
		}

	}
	else if ( sceneRefName == "collection_event_ref" )
	{
		
		
		if ( itemType == eItemType.emote_icon )
		{
			fileLevel.sceneRefOrigin += <0, 105, -10> 
		}
		else if ( itemType == eItemType.weapon_charm )
		{
			fileLevel.sceneRefOrigin += <0, 0, -10>
		}
		else if ( itemType == eItemType.account_currency )
		{
			fileLevel.sceneRefOrigin += <0, 0, -10>
		}
		else if ( itemType == eItemType.account_pack )
		{
			fileLevel.sceneRefOrigin += <0, 0, -10>
		}
		else if ( itemType == eItemType.voucher )
		{
			fileLevel.sceneRefOrigin += <0, 0, -10>
		}

		else if ( itemType == eItemType.character_execution )
		{
			fileLevel.sceneRefOrigin += <0, 0, -20>
			scale *= 0.8
		}
	}

	if ( showLow )
	{
		if ( sceneRefName == "battlepass_center_ref" )
			fileLevel.sceneRefOrigin += <0, 0, -10.5>
		else if (sceneRefName == "collection_event_ref")
			fileLevel.sceneRefOrigin += <0, 0, -10.5>
		else
			fileLevel.sceneRefOrigin += <0, 0, -2>
	}

	if ( sceneRefName == "battlepass_right_ref" || sceneRefName == "battlepass_center_ref" )
	{
		MoveLightsToSceneRef( sceneRef )
	}
	else if ( sceneRefName == "collection_event_ref" )
	{
		
		if ( itemType == eItemType.character_skin )
		{
			fileLevel.sceneRefOrigin += <0, 0, -10>
		}
		else if ( itemType == eItemType.gladiator_card_stance || itemType == eItemType.gladiator_card_frame )
		{
			fileLevel.sceneRefOrigin += <0, 0, 1>
			scale *= 0.8
		}
		else if ( itemType == eItemType.character_emote )
		{
			
			scale *= 0.58
			showEmoteBase = false
		}
	}

#if PC_PROG_NX_UI
	if ( sceneRefName == "customize_character_emote_quip_ref" )
	{
		
		fileLevel.sceneRefOrigin += <10, 0, 0>
	}

	if ( sceneRefName == "customize_character_emotes_ref" )
	{
		if ( !isNXHH )
		{
			
			fileLevel.sceneRefOrigin += <0, 60, 20>
		}
		else
		{
			fileLevel.sceneRefOrigin += <0, -80, 8>
		}
	}

	if ( sceneRefName == "collection_event_ref" )
	{
		if ( itemType == eItemType.emote_icon )
		{
			if( isNXHH )
			{
				fileLevel.sceneRefOrigin += <15, -50, 30>
			}
			else
			{
				fileLevel.sceneRefOrigin += <5, -50, 30>
			}

		}
	}
#endif

	

	if ( isMilestoneEvent )
	{
		fileLevel.sceneRefOrigin += <-45, 0, 0>
		if ( !showLow )
			fileLevel.sceneRefOrigin += <0, 0, 5>

		if ( itemType == eItemType.gladiator_card_stat_tracker )
			fileLevel.sceneRefOrigin += <0, 0, 0>
		else if ( itemType == eItemType.gladiator_card_stance )
			fileLevel.sceneRefOrigin += <0, 0, 0>
		else if ( itemType == eItemType.gladiator_card_frame )
			fileLevel.sceneRefOrigin += <0, 0, 0>
		else if ( itemType == eItemType.emote_icon )
		{
			fileLevel.sceneRefOrigin += <0, -50, 30>

#if PC_PROG_NX_UI
			if ( isNXHH )
				fileLevel.sceneRefOrigin += <5, -50, -35>
			else
				fileLevel.sceneRefOrigin += <-2, 50, -36>
#endif

		}
		else if ( itemType == eItemType.character_emote )
		{
			fileLevel.sceneRefOrigin += <0, 0, -25>
			scale *= 1.25
		}
		else if ( itemType == eItemType.weapon_skin )
		{
			fileLevel.sceneRefOrigin += <-10, 0, -3>
			scale *= 1.25
		}
		else if ( itemType == eItemType.character_skin )
			fileLevel.sceneRefOrigin += <0, 0, 0>
		else if ( itemType == eItemType.melee_skin )
		{
			
			fileLevel.sceneRefOrigin += <0, 0, 0>
			scale *= 1.5
		}
	}
		
#if PC_PROG_NX_UI
		
		if ( isNXHH && itemType == eItemType.emote_icon )
		{
			fileLevel.sceneRefOrigin += <-15, 80, 0>
		}
#endif

	
	if ( isBattlepassMilestone )
	{
		fileLevel.sceneRefOrigin += <-14, 0, 2.5> 

		if ( itemType == eItemType.character_skin )
		{
			scale *= 1.05
		}
		else if ( itemType == eItemType.gladiator_card_frame )
		{
			fileLevel.sceneRefOrigin += <0, 0, 0>
			scale *= 0.90
		}
		else if ( itemType == eItemType.character_emote )
		{
			fileLevel.sceneRefOrigin += <0, 0, 2.5>
		}
		else if ( itemType == eItemType.skydive_emote || itemType == eItemType.weapon_skin )
		{
			fileLevel.sceneRefOrigin += <1, 0, 0.5>
			scale *= 1.5
		}
	}

	fileLevel.sceneRefOrigin += offset

	fileLevel.sceneRefAngles = sceneRef.GetAngles()
	ShowBattlepassItem( flav, level, scale, loadscreenPreviewBox, shouldPlayAudioPreview, showLow, showEmoteBase, useMenuZoomOffset )

	
	

	
}

void function MoveLightsToSceneRef( entity sceneRef )
{
	foreach ( light in fileLevel.stationaryLights )
	{
		light.SetOrigin( sceneRef.GetOrigin() + fileLevel.stationaryLightOffsets[ light ] )
		light.SetTweakLightOrigin( sceneRef.GetOrigin() + fileLevel.stationaryLightOffsets[ light ] )
	}
}

void function ShowBattlepassItem( ItemFlavor item, int level, float scale, var loadscreenPreviewBox, bool shouldPlayAudioPreview, bool showLow = false, bool showEmoteBase = true, bool useMenuZoomOffset = true )
{
	fileLevel.loadscreenPreviewBox = loadscreenPreviewBox 

	ClearBattlePassItem()

	fileLevel.loadscreenPreviewBox = loadscreenPreviewBox

	int itemType = ItemFlavor_GetType( item )

	switch ( itemType )
	{
		case eItemType.account_currency:
		case eItemType.account_currency_bundle:
			ShowBattlePassItem_Currency( item, scale )
			break

		case eItemType.account_pack:
			ShowBattlePassItem_ApexPack( item, scale )
			break

		case eItemType.character_skin:
			ShowBattlePassItem_CharacterSkin( item, scale )
			break

		case eItemType.character_execution:
			ShowBattlePassItem_Execution( item, scale )
			break

		case eItemType.weapon_skin:
			asset video = WeaponSkin_GetVideo( item )
			if ( video != $"" )
				ShowBattlePassItem_WeaponSkinVideo( item, scale, video )
			else
				ShowBattlePassItem_WeaponSkin( item, scale )
			break

		case eItemType.weapon_charm:
			ShowBattlePassItem_WeaponCharm( item, scale )
			break

		case eItemType.artifact_component_blade:
		case eItemType.artifact_component_power_source:
		case eItemType.artifact_component_theme:
		case eItemType.melee_skin:
			ShowBattlePassItem_MeleeSkin( item, scale )
			break

		case eItemType.gladiator_card_stance:
		case eItemType.gladiator_card_frame:
			ShowBattlePassItem_Banner( item, scale )
			break

		case eItemType.gladiator_card_intro_quip:
		case eItemType.gladiator_card_kill_quip:
			ShowBattlePassItem_Quip( item, scale, shouldPlayAudioPreview )
			break

		case eItemType.gladiator_card_stat_tracker:
			ShowBattlePassItem_StatTracker( item, scale )
			break

		case eItemType.voucher:
			ShowBattlePassItem_Voucher( item, scale )
			break
		case eItemType.gladiator_card_badge:
			ShowBattlePassItem_Badge( item, scale, level )
			break

		case eItemType.account_flag:
			ShowBattlePassItem_AccountFlag( item )
			break

		case eItemType.battlepass:
		case eItemType.image_2d:
			ShowBattlePassItem_Image2D( item )
			break

		case eItemType.music_pack:
			ShowBattlePassItem_MusicPack( item, scale, shouldPlayAudioPreview )
			break

		case eItemType.loadscreen:
			ShowBattlePassItem_Loadscreen( item, scale )
			break

		case eItemType.radio_play:
			ShowBattlePassItem_RadioPlay( item, scale )
			break

		case eItemType.skydive_emote:
			ShowBattlePassItem_SkydiveEmote( item, scale )
			break

		case eItemType.emote_icon:
			ShowBattlePassItem_EmoteIcon( item, scale, showLow )
			break

		case eItemType.character_emote:
			ShowBattlePassItem_Emote( item, scale, showEmoteBase, useMenuZoomOffset )
			break

		case eItemType.quest_mission:
			ShowBattlePassItem_QuestMission( item, scale )
			break


		case eItemType.quest_comic:
			ShowBattlePassItem_QuestComicPage( item, scale )
			break


		case eItemType.battlepass_purchased_xp:
			thread ShowBattlePassItem_Level( item, scale )
			break

		case eItemType.character:
			thread ShowBattlePassItem_Character( item, scale )
			break

		case eItemType.sticker:
			ShowBattlePassItem_Sticker( item )
			break







		case eItemType.artifact_component_activation_emote:
		case eItemType.artifact_component_deathbox:
			ShowBattlePassItem_ArtifactBink( item, scale )
			break


		case eItemType.reward_set_tracker:
			ShowBattlePassItem_RewardSetTracker( item )
			break

		default:
			Warning( "Battle Pass reward item type not supported: " + DEV_GetEnumStringSafe( "eItemType", itemType ) )
			ShowBattlePassItem_Unknown( item, scale )
			break
	}
}



void function ClearBattlePassItem()
{
	foreach ( model in fileLevel.models )
	{
		if ( IsValid( model ) )
		{
			foreach ( entity ent in GetEntityAndAllChildren( model ) )
			{
				if ( IsValid( ent ) )
					ent.Destroy()
			}
		}
	}
	fileLevel.models.clear()

	foreach ( fx in fileLevel.fxs )
	{
		if ( EffectDoesExist( fx ) )
		{
			EffectStop( fx, true, true )
		}
	}
	fileLevel.fxs.clear()

	if ( IsValid( fileLevel.mover ) )
		fileLevel.mover.Destroy()

	CleanupNestedGladiatorCard( fileLevel.bannerHandle )

	if ( fileLevel.rui != null )
		RuiDestroyIfAlive( fileLevel.rui )

	if ( fileLevel.topo != null )
	{
		RuiTopology_Destroy( fileLevel.topo )
		fileLevel.topo = null
	}

	if ( fileLevel.videoChannel != -1 )
	{
		ReleaseVideoChannel( fileLevel.videoChannel )
		fileLevel.videoChannel = -1
	}

	if ( fileLevel.playingPreviewAlias != "" )
		StopSoundOnEntity( GetLocalClientPlayer(), fileLevel.playingPreviewAlias )

	if ( fileLevel.loadscreenPreviewBox != null && IsValid( fileLevel.loadscreenPreviewBox ) )
	{
		UpdateLoadscreenPreviewMaterial( fileLevel.loadscreenPreviewBox, null, 0 )
		fileLevel.loadscreenPreviewBox = null
	}
}

void function ShowBattlePassItem_ApexPack( ItemFlavor item, float scale )
{
	vector origin = fileLevel.sceneRefOrigin + <0, 0, 10.0>
	vector angles = fileLevel.sceneRefAngles

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	asset ornull tickAsset = GRXPack_GetTickModel( item )
	string tickSkin = GRXPack_GetTickModelSkin( item )

	if ( tickAsset == null || tickSkin == "" )
		return

	expect asset( tickAsset )

	entity model    = CreateClientSidePropDynamic( origin, AnglesCompose( angles, <0, 135, 0> ), tickAsset )
	model.MakeSafeForUIScriptHack()
	model.SetModelScale( scale * 0.60 )
	model.SetParent( mover )
	model.SetSkin( model.GetSkinIndexByName( tickSkin ) )

	mover.NonPhysicsRotate( <0, 0, -1>, fileLevel.rotateSpeed )

	vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
	thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )

	fileLevel.mover = mover
	fileLevel.models.append( model )
}


void function ShowBattlePassItem_CharacterSkin( ItemFlavor item, float scale )
{
	
	vector season22BattlePassOffset = fileLevel.sceneRefName == "battlepass_center_ref" ? <-1.0, 60.0, 0.0> : <0.0, 0.0, 0.0>

	ItemFlavor char = CharacterSkin_GetCharacterFlavor( item )
	vector origin   = fileLevel.sceneRefOrigin + season22BattlePassOffset + <0, 0, 4.0> - 0.6 * CharacterClass_GetMenuZoomOffset( char )
	vector angles   = fileLevel.sceneRefAngles

	vector previewVerticalOffset = GetGlobalSettingsVector( ItemFlavor_GetAsset( item ), "previewOffset" )
	origin += previewVerticalOffset

	vector previewAnglesOffset = GetGlobalSettingsVector( ItemFlavor_GetAsset( item ), "previewRotation" )
	angles += previewAnglesOffset

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamic( origin, angles, $"mdl/dev/empty_model.rmdl" )


	
	if ( fileLevel.sceneRefName == "battlepass_center_ref" )
		model.SetLightingOrigin( <11328, 2612, 28> ) 


	CharacterSkin_Apply( model, item )
	model.MakeSafeForUIScriptHack()
	model.SetModelScale( scale * 0.75 )
	model.SetParent( mover )
	
	thread MoverPendulum( mover )

	thread PlayAnim( model, "ACT_MP_MENU_LOOT_CEREMONY_IDLE", mover )

	vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
	thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )

	fileLevel.mover = mover
	fileLevel.models.append( model )
}

void function MoverPendulum( entity mover )
{
	vector baseAngles = mover.GetAngles()
	bool firstTime    = false
	float dir         = 1
	float delta       = 30.0
	const float MOVE_TIME = 10.0

	while ( IsValid( mover ) )
	{
		float scalar   = firstTime ? 0.5 : 1.0
		float moveTime = MOVE_TIME * scalar
		mover.NonPhysicsRotateTo( baseAngles - (dir * <0, delta, 0>), moveTime, 0.5 * scalar, 0.5 * scalar )
		firstTime = false
		dir *= -1
		wait moveTime
	}
}

void function ShowBattlePassItem_Execution( ItemFlavor item, float scale )
{
	const float BATTLEPASS_EXECUTION_Z_OFFSET = 12.0
	const vector BATTLEPASS_EXECUTION_LOCAL_ANGLES = <0, 15, 0>
	const float BATTLEPASS_EXECUTION_SCALE = 0.8

	
	ItemFlavor attackerCharacter = CharacterExecution_GetCharacterFlavor( item )
	ItemFlavor characterSkin     = LoadoutSlot_GetItemFlavor( LocalClientEHI(), Loadout_CharacterSkin( attackerCharacter ) )

	asset attackerAnimSeq = CharacterExecution_GetAttackerPreviewAnimSeq( item )
	asset victimAnimSeq   = CharacterExecution_GetVictimPreviewAnimSeq( item )

	
	vector origin = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_EXECUTION_Z_OFFSET>
	vector angles = AnglesCompose( fileLevel.sceneRefAngles, BATTLEPASS_EXECUTION_LOCAL_ANGLES )

	entity mover         = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	entity attackerModel = CreateClientSidePropDynamic( origin, angles, $"mdl/dev/empty_model.rmdl" )
	entity victimModel   = CreateClientSidePropDynamic( origin, angles, $"mdl/dev/empty_model.rmdl" )

	CharacterSkin_Apply( attackerModel, characterSkin )
	victimModel.SetModel( $"mdl/humans/class/medium/dummy_v20_base_w.rmdl" )

	
	bool attackerHasSequence = attackerModel.Anim_HasSequence( attackerAnimSeq )
	bool victimHasSequence   = victimModel.Anim_HasSequence( victimAnimSeq )

	if ( !attackerHasSequence || !victimHasSequence )
	{
		asset attackerPlayerSettings = CharacterClass_GetSetFile( attackerCharacter )
		string attackerRigWeight     = GetGlobalSettingsString( attackerPlayerSettings, "bodyModelRigWeight" )
		string attackerAnim          = "mp_pt_execution_" + attackerRigWeight + "_attacker_loot"

		attackerModel.Anim_Play( attackerAnim )
		victimModel.Anim_Play( "mp_pt_execution_default_victim_loot" )
		Warning( "Couldn't find menu idles for execution reward: " + DEV_DescItemFlavor( item ) + ". Using fallback anims." )
		if ( !attackerHasSequence )
			Warning( "ATTACKER could not find sequence: " + attackerAnimSeq )
		if ( !victimHasSequence )
			Warning( "VICTIM could not find sequence: " + victimAnimSeq )
	}
	else
	{
		attackerModel.Anim_Play( attackerAnimSeq )
		victimModel.Anim_Play( victimAnimSeq )
	}

	mover.MakeSafeForUIScriptHack()

	attackerModel.MakeSafeForUIScriptHack()
	attackerModel.SetParent( mover )

	victimModel.MakeSafeForUIScriptHack()
	victimModel.SetParent( mover )

	
	attackerModel.SetModelScale( scale * BATTLEPASS_EXECUTION_SCALE )
	victimModel.SetModelScale( scale * BATTLEPASS_EXECUTION_SCALE )

	vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
	thread FlashMenuModel( attackerModel, eMenuModelFlashType.BATTLEPASS, flashColor )
	thread FlashMenuModel( victimModel, eMenuModelFlashType.BATTLEPASS, flashColor )

	fileLevel.mover = mover
	fileLevel.models.append( attackerModel )
	fileLevel.models.append( victimModel )
}


void function ShowBattlePassItem_WeaponSkin( ItemFlavor item, float scale )
{
	const vector BATTLEPASS_WEAPON_SKIN_LOCAL_ANGLES = <5, -45, 0>

	vector origin = fileLevel.sceneRefOrigin + <0, 0, 29.0>
	vector angles = fileLevel.sceneRefAngles

	
	ItemFlavor weaponFlavor = WeaponSkin_GetWeaponFlavor( item )

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	float weaponItemScale = WeaponItemFlavor_GetBattlePassScale( weaponFlavor )
	entity weaponModel    = CreateClientSidePropDynamic( origin, AnglesCompose( angles, BATTLEPASS_WEAPON_SKIN_LOCAL_ANGLES ), $"mdl/dev/empty_model.rmdl" )
	WeaponCosmetics_Apply( weaponModel, item, null )

	bool isReactive = WeaponSkin_DoesReactToKills( item )
	if ( isReactive )
		MenuWeaponModel_ApplyReactiveSkinBodyGroup( item, weaponFlavor, weaponModel )
	else
		ShowDefaultBodygroupsOnFakeWeapon( weaponModel, WeaponItemFlavor_GetClassname( weaponFlavor ) )

	MenuWeaponModel_ClearReactiveEffects( weaponModel )
	if ( isReactive )
		MenuWeaponModel_StartReactiveEffects( weaponModel, item )

	weaponModel.MakeSafeForUIScriptHack()
	weaponModel.SetVisibleForLocalPlayer( 0 )
	weaponModel.Anim_SetPaused( true )
	weaponModel.SetModelScale( scale * weaponItemScale * 0.75 )
	weaponModel.SetParent( mover )

	
	weaponModel.SetLocalOrigin( GetAttachmentOriginOffset( weaponModel, "MENU_ROTATE", BATTLEPASS_WEAPON_SKIN_LOCAL_ANGLES ) )
	weaponModel.SetLocalAngles( BATTLEPASS_WEAPON_SKIN_LOCAL_ANGLES )

	mover.NonPhysicsRotate( <0, 0, -1>, fileLevel.rotateSpeed )

	vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
	thread FlashMenuModel( weaponModel, eMenuModelFlashType.BATTLEPASS, flashColor )

	fileLevel.mover = mover
	fileLevel.models.append( weaponModel )
}


void function ShowBattlePassItem_WeaponCharm( ItemFlavor item, float scale )
{
	vector origin = fileLevel.sceneRefOrigin + <0, 0, 42>
	vector angles = fileLevel.sceneRefAngles

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamicCharm( origin, AnglesCompose( angles, <0, 270, 0> ), WeaponCharm_GetCharmModel( item ) )
	model.MakeSafeForUIScriptHack()
	model.SetModelScale( scale * 18 )
	model.SetParent( mover )
	thread MoverPendulum( mover )

	vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
	thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )

	fileLevel.mover = mover
	fileLevel.models.append( model )
}


void function ShowBattlePassItem_MeleeSkin( ItemFlavor item, float scale )
{
	vector origin = fileLevel.sceneRefOrigin + <0, 0, 29.0>
	vector angles = fileLevel.sceneRefAngles

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	bool isArtifact = false
	int setIndex = -1
	array< ItemFlavor > setItems = []
	array< ItemFlavor > emptySetItems = Artifacts_GetSetItems( eArtifactSetIndex._EMPTY )
	bool isBlade = ItemFlavor_GetType( item ) == eItemType.artifact_component_blade
	bool isPowerSource = ItemFlavor_GetType( item ) == eItemType.artifact_component_power_source
	bool isTheme = ItemFlavor_GetType( item ) == eItemType.artifact_component_theme
	if ( isBlade || isPowerSource || isTheme )
	{
		setIndex = Artifacts_GetSetIndex( item )
		setItems = Artifacts_GetSetItems( setIndex )
		item = GetItemFlavorByGUID( ARTIFACT_CONFIGURATION_PTR_0_GUID )
		isArtifact = true
	}

	vector extraRotation = MeleeSkin_GetMenuModelRotation( item )
	entity model         = CreateClientSidePropDynamic( origin, AnglesCompose( angles, extraRotation ), MeleeSkin_GetMenuModel( item ) )
	model.MakeSafeForUIScriptHack()
	model.SetVisibleForLocalPlayer( 0 )

	if ( isArtifact )
	{
		
		
		

		ItemFlavor bladeComponent = setItems[ eArtifactComponentType.BLADE ] 
		ItemFlavor themeComponent = isBlade ? emptySetItems[ eArtifactComponentType.THEME ] : setItems[ eArtifactComponentType.THEME ] 
		ItemFlavor powerSourceComponent = isPowerSource ? setItems[ eArtifactComponentType.POWER_SOURCE ] : emptySetItems[ eArtifactComponentType.POWER_SOURCE ] 

		Artifacts_Loadouts_ApplyModelForSet( model, setIndex )
		Artifacts_Loadouts_PreviewBladeAndPowerSource( model, bladeComponent, powerSourceComponent )
		Artifacts_Loadouts_PreviewTheme( model, themeComponent )
	}

	asset animSeq = MeleeSkin_GetMenuAnimSeq( item )
	if ( animSeq != $"" )
		model.Anim_Play( animSeq )

	model.SetModelScale( scale * WeaponItemFlavor_GetBattlePassScale( item ) )
	model.SetParent( mover )

	model.SetLocalOrigin( GetAttachmentOriginOffset( model, "MENU_ROTATE", extraRotation ) )
	model.SetLocalAngles( extraRotation )

	mover.NonPhysicsRotate( <0, 0, -1>, fileLevel.rotateSpeed )

	vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
	thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )

	fileLevel.mover = mover
	fileLevel.models.append( model )
}


void function ShowBattlePassItem_Banner( ItemFlavor item, float scale )
{
	int itemType = ItemFlavor_GetType( item )
	Assert( itemType == eItemType.gladiator_card_frame || itemType == eItemType.gladiator_card_stance )

	const float BATTLEPASS_BANNER_WIDTH = 528.0
	const float BATTLEPASS_BANNER_HEIGHT = 912.0
	const float BATTLEPASS_BANNER_SCALE = 0.08
	const float BATTLEPASS_BANNER_Z_OFFSET = -4.0

	entity player = GetLocalClientPlayer()
	vector origin = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_BANNER_Z_OFFSET>
	vector angles = AnglesCompose( fileLevel.sceneRefAngles, <0, 180, 0> )

	float width  = scale * BATTLEPASS_BANNER_WIDTH * BATTLEPASS_BANNER_SCALE
	float height = scale * BATTLEPASS_BANNER_HEIGHT * BATTLEPASS_BANNER_SCALE

	var topo = CreateRUITopology_Worldspace( origin + <0, 0, height * 0.5>, angles, width, height )
	var rui  = RuiCreate( $"ui/loot_ceremony_glad_card.rpak", topo, RUI_DRAW_VIEW_MODEL, 0 )

	int gcardPresentation
	if ( itemType == eItemType.gladiator_card_frame )
		gcardPresentation = eGladCardPresentation.FRONT_CLEAN
	else
		gcardPresentation = eGladCardPresentation.FRONT_STANCE_ONLY

	NestedGladiatorCardHandle nestedGCHandleFront = CreateNestedGladiatorCard( rui, "card", eGladCardDisplaySituation.MENU_LOOT_CEREMONY_ANIMATED, gcardPresentation )
	ChangeNestedGladiatorCardOwner( nestedGCHandleFront, ToEHI( player ) )

	if ( itemType == eItemType.gladiator_card_frame )
	{
		ItemFlavor ornull character = GladiatorCardFrame_GetCharacterFlavor( item )
		if ( character == null )
			character = GetRandomGoodItemFlavorForLoadoutSlot( ToEHI( player ), Loadout_Character() )

		expect ItemFlavor( character )
		SetNestedGladiatorCardOverrideCharacter( nestedGCHandleFront, character )
		SetNestedGladiatorCardOverrideFrame( nestedGCHandleFront, item )
	}
	else
	{
		ItemFlavor character = GladiatorCardStance_GetCharacterFlavor( item )
		SetNestedGladiatorCardOverrideCharacter( nestedGCHandleFront, character )
		SetNestedGladiatorCardOverrideStance( nestedGCHandleFront, item )

		ItemFlavor characterDefaultFrame = GetDefaultItemFlavorForLoadoutSlot( EHI_null, Loadout_GladiatorCardFrame( character ) )
		SetNestedGladiatorCardOverrideFrame( nestedGCHandleFront, characterDefaultFrame ) 
	}

	RuiSetBool( rui, "battlepass", true )
	RuiSetInt( rui, "rarity", ItemFlavor_GetQuality( item ) )

	fileLevel.topo = topo
	fileLevel.rui = rui
	fileLevel.bannerHandle = nestedGCHandleFront
}

void function ShowBattlePassItem_EmoteIcon( ItemFlavor item, float scale, bool showLow )
{

	asset EMOTE_ICON_BASE_MODEL = HOLO_SPRAY_BASE 

	vector angles = fileLevel.sceneRefAngles

#if PC_PROG_NX_UI
		vector origin = fileLevel.sceneRefOrigin - (AnglesToForward( angles ) * ( (1.0 - scale) ) ) + (AnglesToRight( angles ) * ((1.0 - scale)) * -28) + <0, 0, -25>
#else
		vector origin = fileLevel.sceneRefOrigin - (AnglesToForward( angles ) * ( (1.0 - scale) * 100) ) + (AnglesToRight( angles ) * ((1.0 - scale) * -12))

		if ( showLow )
			origin -= <0,0,32>
#endif

	entity model = CreateClientSidePropDynamic( origin, angles, EMOTE_ICON_BASE_MODEL )

	model.MakeSafeForUIScriptHack()
	model.SetModelScale( scale )
	model.SetDoDestroyCallback( true )

	thread CreateClientSideEmoteIcon( model, ItemFlavor_GetGUID( item ), Time(), true )

	vector fwd = AnglesToForward( angles )
	vector backerOrg = origin + <0,0,64>

	entity backer = CreateClientsideScriptMover( $"mdl/levels_terrain/mp_lobby/holospray_backdrop_godray_01.rmdl", backerOrg - (fwd*5), angles + <180,0,0> )
	backer.MakeSafeForUIScriptHack()
	backer.SetModelScale( 4.0 )
	backer.SetParent( model )

	fileLevel.models.append( model )
}


void function ShowBattlePassItem_Emote( ItemFlavor item, float scale, bool showBase = true, bool useMenuZoomOffset = true )
{
	ItemFlavor ornull char = CharacterQuip_GetCharacterFlavor( item )

	if ( char == null )
		char = LoadoutSlot_GetItemFlavor( LocalClientEHI(), Loadout_Character() )

	expect ItemFlavor( char )

	ItemFlavor skin = LoadoutSlot_GetItemFlavor( LocalClientEHI(), Loadout_CharacterSkin( char ) )

	vector angles = fileLevel.sceneRefAngles
	vector origin = fileLevel.sceneRefOrigin

	if ( useMenuZoomOffset )
		origin = origin - CharacterClass_GetMenuZoomOffset( char )

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamic( origin, angles, $"mdl/dev/empty_model.rmdl" )
	CharacterSkin_Apply( model, skin )
	model.MakeSafeForUIScriptHack()
	model.SetModelScale( scale )
	model.SetParent( mover )

	int fx
	if ( showBase )
	{
		fx = StartParticleEffectInWorldWithHandle( GetParticleSystemIndex( CHARACTER_BASE_EFFECT ), mover.GetOrigin() + <0.0, 0.0, -1.2>, <0.0, 0.0, 0.0> )
		EffectSetControlPointVector( fx, 1, ItemFlavor_GetQualityColor( item ) )
	}

	thread ModelPerformEmote( model, item, mover, false )

	fileLevel.mover = mover
	fileLevel.models.append( model )
	if ( showBase )
		fileLevel.fxs.append( fx )
}


void function ShowBattlePassItem_Quip( ItemFlavor item, float scale, bool shouldPlayAudioPreview )
{
	int itemType = ItemFlavor_GetType( item )
	Assert( itemType == eItemType.gladiator_card_intro_quip || itemType == eItemType.gladiator_card_kill_quip )

	const float BATTLEPASS_QUIP_WIDTH = 390.0
	const float BATTLEPASS_QUIP_HEIGHT = 208.0
	const float BATTLEPASS_QUIP_SCALE_NX = 0.11
	const float BATTLEPASS_QUIP_SCALE = 0.091
	const float BATTLEPASS_QUIP_Z_OFFSET = 20.5
	const asset BATTLEPASS_QUIP_BG_MODEL = $"mdl/menu/loot_ceremony_quip_bg.rmdl"

	vector origin = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_QUIP_Z_OFFSET>

	vector angles        = fileLevel.sceneRefAngles
	vector placardAngles = VectorToAngles( AnglesToForward( angles ) * -1 )

	
#if PC_PROG_NX_UI
		float width  = scale * BATTLEPASS_QUIP_WIDTH * BATTLEPASS_QUIP_SCALE_NX
		float height = scale * BATTLEPASS_QUIP_HEIGHT * BATTLEPASS_QUIP_SCALE_NX
#else
		float width  = scale * BATTLEPASS_QUIP_WIDTH * BATTLEPASS_QUIP_SCALE
		float height = scale * BATTLEPASS_QUIP_HEIGHT * BATTLEPASS_QUIP_SCALE
#endif

	entity model = CreateClientSidePropDynamic( origin, angles, BATTLEPASS_QUIP_BG_MODEL )
	model.MakeSafeForUIScriptHack()

#if PC_PROG_NX_UI
		model.SetModelScale( scale * BATTLEPASS_QUIP_SCALE_NX )
#else
		model.SetModelScale( scale * BATTLEPASS_QUIP_SCALE )
#endif

	var topo         = CreateRUITopology_Worldspace( origin + <0, 0, (height * 0.5)>, placardAngles, width, height )
	var rui
	ItemFlavor quipCharacter
	string labelText
	string quipAlias = ""

	if ( itemType == eItemType.gladiator_card_intro_quip )
	{
		
		rui = RuiCreate( $"ui/loot_reward_intro_quip.rpak", topo, RUI_DRAW_WORLD, 0 )
		quipCharacter = CharacterIntroQuip_GetCharacterFlavor( item )
		labelText = "#LOOT_QUIP_INTRO"
		quipAlias = CharacterIntroQuip_GetVoiceSoundEvent( item )
	}
	else
	{
		
		rui = RuiCreate( $"ui/loot_reward_kill_quip.rpak", topo, RUI_DRAW_WORLD, 0 )
		quipCharacter = CharacterKillQuip_GetCharacterFlavor( item )
		labelText = "#LOOT_QUIP_KILL"
		quipAlias = CharacterKillQuip_GetVictimVoiceSoundEvent( item )
	}

	RuiSetBool( rui, "isVisible", true )
	RuiSetBool( rui, "battlepass", true )
	RuiSetInt( rui, "rarity", ItemFlavor_GetQuality( item ) )
	RuiSetImage( rui, "portraitImage", CharacterClass_GetGalleryPortrait( quipCharacter ) )
	RuiSetString( rui, "quipTypeText", labelText )
	RuiTrackFloat( rui, "level", null, RUI_TRACK_SOUND_METER, 0 )

	fileLevel.models.append( model )
	fileLevel.topo = topo
	fileLevel.rui = rui

	
	if ( quipAlias != "" && shouldPlayAudioPreview )
	{
		fileLevel.playingPreviewAlias = quipAlias
		EmitSoundOnEntity( GetLocalClientPlayer(), quipAlias )
	}
}


void function ShowBattlePassItem_StatTracker( ItemFlavor item, float scale )
{
	const float BATTLEPASS_STAT_TRACKER_WIDTH = 594.0
	const float BATTLEPASS_STAT_TRACKER_HEIGHT = 230.0
	const float BATTLEPASS_STAT_TRACKER_SCALE = 0.06
	const asset BATTLEPASS_STAT_TRACKER_BG_MODEL = $"mdl/menu/loot_ceremony_stat_tracker_bg.rmdl"

	vector origin        = fileLevel.sceneRefOrigin + <0, 0, 23>
	vector angles        = fileLevel.sceneRefAngles
	vector placardAngles = VectorToAngles( AnglesToForward( angles ) * -1 )

	
	float width = BATTLEPASS_STAT_TRACKER_WIDTH
	width *= scale
	width *= BATTLEPASS_STAT_TRACKER_SCALE
	float height = BATTLEPASS_STAT_TRACKER_HEIGHT
	height *= scale
	height *= BATTLEPASS_STAT_TRACKER_SCALE

	var topo = CreateRUITopology_Worldspace( origin + <0, 0, (height * 0.5)>, placardAngles, width, height )
	var rui  = RuiCreate( $"ui/loot_ceremony_stat_tracker.rpak", topo, RUI_DRAW_WORLD, 0 )

	entity model = CreateClientSidePropDynamic( origin, angles, BATTLEPASS_STAT_TRACKER_BG_MODEL )
	model.MakeSafeForUIScriptHack()
	model.SetModelScale( scale * BATTLEPASS_STAT_TRACKER_SCALE )

	ItemFlavor ornull character = GladiatorCardStatTracker_GetCharacterFlavor( item )
	if ( character == null ) 
		character = LoadoutSlot_GetItemFlavor( LocalClientEHI(), Loadout_Character() )

	RuiSetBool( rui, "isVisible", true )
	RuiSetBool( rui, "battlepass", true )
	UpdateRuiWithStatTrackerData( rui, "tracker", LocalClientEHI(), character, -1, item, null, true )
	RuiSetColorAlpha( rui, "trackerColor0", GladiatorCardStatTracker_GetColor0( item ), 1.0 )
	RuiSetInt( rui, "rarity", ItemFlavor_GetQuality( item ) )

	fileLevel.models.append( model )
	fileLevel.topo = topo
	fileLevel.rui = rui
}


void function ShowBattlePassItem_Badge( ItemFlavor item, float scale, int level )
{
	const float BATTLEPASS_BADGE_WIDTH = 670.0
	const float BATTLEPASS_BADGE_HEIGHT = 670.0
	const float BATTLEPASS_BADGE_SCALE = 0.06

	vector origin        = fileLevel.sceneRefOrigin + <0, 0, 30>
	vector angles        = fileLevel.sceneRefAngles
	vector placardAngles = VectorToAngles( AnglesToForward( angles ) * -1 )

	float width  = scale * BATTLEPASS_BADGE_WIDTH * BATTLEPASS_BADGE_SCALE
	float height = scale * BATTLEPASS_BADGE_HEIGHT * BATTLEPASS_BADGE_SCALE

	var topo = CreateRUITopology_Worldspace( origin, placardAngles, width, height )
	var rui  = RuiCreate( $"ui/world_space_badge.rpak", topo, RUI_DRAW_VIEW_MODEL, 0 )
	ItemFlavor dummy
	CreateNestedGladiatorCardBadge( rui, "badge", LocalClientEHI(), item, 0, dummy, level == -1 ? 0 : level )
	RuiSetBool( rui, "isVisible", true )

		RuiSetBool( rui, "introComplete", true )
		RuiSetFloat( rui, "bloomScale", 0.0 )
		RuiSetBool( rui, "battlepass", false )



	fileLevel.topo = topo
	fileLevel.rui = rui
}

void function ShowBattlePassItem_AccountFlag( ItemFlavor item ) 
{
	int type = ItemFlavor_GetType( item )
	UISize screenSize = GetScreenSize()

	
	float heightOffset = 100.0 / 1080.0
	
	vector origin = <140, screenSize.height * heightOffset * -1, 0>

	const asset ruiAsset = $"ui/world_space_image2d.rpak"

	var topo = RuiTopology_CreatePlane( origin, <screenSize.width, 0, 0>, <0, screenSize.height, 0>, true )
	var rui = RuiCreate( ruiAsset, topo, RUI_DRAW_POSTEFFECTS, 0 )

	RuiSetImage( rui, "image2d", $"" )

	fileLevel.topo = topo
	fileLevel.rui = rui

	
	float baseImageScale = 0.7
	float baseImageWidth = 740.0
	float baseImageHeight = 740.0
	bool disableLoadIcon = false

	asset image2d = AccountFlag_GetRewardIcon( item )
	RuiSetImage( rui, "image2d", image2d )
	RuiSetFloat( rui, "imageWidth", baseImageWidth )
	RuiSetFloat( rui, "imageHeight", baseImageHeight )
	RuiSetFloat( rui, "imageScale", baseImageScale )
	RuiSetBool( rui, "disableLoadIcon", disableLoadIcon )
}


const float RUI_IMAGE_2D_SCALE = 0.7
const float RUI_IMAGE_2D_WIDTH = 1570.0
const float RUI_IMAGE_2D_HEIGHT = 740.0
const float ORIGIN_IMAGE_2D_YOFFSET = 100.0
const float RUI_BP_IMAGE_WIDTH = 1177.6
const float RUI_BP_IMAGE_HEIGHT = 555.0
const float ORIGIN_BP_IMAGE_XOFFSET = 480.0
const float ORIGIN_BP_IMAGE_YOFFSET = 0.0
void function ShowBattlePassItem_Image2D( ItemFlavor item )
{
	int type = ItemFlavor_GetType( item )
	UISize screenSize = GetScreenSize()

	
	float yOffset = type == eItemType.image_2d ? ORIGIN_IMAGE_2D_YOFFSET : ORIGIN_BP_IMAGE_YOFFSET
	float heightOffset = yOffset / 1080.0
	
	float xPos = 0.0
	if ( type == eItemType.battlepass && fileLevel.sceneRefName == "battlepass_right_ref" )
		xPos += ORIGIN_BP_IMAGE_XOFFSET * screenSize.width / 1920.0
	vector origin = <xPos, screenSize.height * heightOffset * -1, 0.0>

	const asset ruiAsset = $"ui/world_space_image2d.rpak"

	var topo = RuiTopology_CreatePlane( origin, <screenSize.width, 0, 0>, <0, screenSize.height, 0>, true )
	var rui = RuiCreate( ruiAsset, topo, RUI_DRAW_POSTEFFECTS, 0 )

	RuiSetImage( rui, "image2d", $"" )

	fileLevel.topo = topo
	fileLevel.rui = rui

	asset image2d
	float baseImageWidth
	float baseImageHeight

	if ( type == eItemType.image_2d )
	{
		image2d = GetImage2DAssetFromItemFlav( item )
		baseImageWidth = RUI_IMAGE_2D_WIDTH
		baseImageHeight = RUI_IMAGE_2D_HEIGHT
	}
	else if ( type == eItemType.battlepass )
	{
		image2d = BattlePass_GetStoreBundleContentsImage( item )
		baseImageWidth = RUI_BP_IMAGE_WIDTH
		baseImageHeight = RUI_BP_IMAGE_HEIGHT
	}
	else
	{
		Assert( false )
		return
	}

	RuiSetImage( rui, "image2d", image2d )
	RuiSetFloat( rui, "imageWidth", baseImageWidth )
	RuiSetFloat( rui, "imageHeight", baseImageHeight )
	RuiSetFloat( rui, "imageScale", RUI_IMAGE_2D_SCALE )
	RuiSetBool( rui, "disableLoadIcon", false )
}

const float RUI_REWARD_SET_TRACKER_Z_OFFSET = 30.0
void function ShowBattlePassItem_RewardSetTracker( ItemFlavor item )
{
	Assert( ItemFlavor_GetType( item ) == eItemType.reward_set_tracker )

	vector origin = fileLevel.sceneRefOrigin + <0, 0, RUI_REWARD_SET_TRACKER_Z_OFFSET>
	vector angles        = fileLevel.sceneRefAngles
	vector placardAngles = VectorToAngles( AnglesToForward( angles ) * -1 )

	var topo = CreateRUITopology_Worldspace( origin, placardAngles, 1920, 1080 )
	var rui  = RuiCreate( $"ui/world_space_image2d.rpak", topo, RUI_DRAW_WORLD, 0 )

	RuiSetImage( rui, "image2d", $"" )

	fileLevel.topo = topo
	fileLevel.rui = rui

	
	float baseImageScale = 0.0625
	float baseImageWidth = 630.0
	float baseImageHeight = 820.0

	RuiSetImage( rui, "image2d", RewardSetTracker_GetDisplayImageAsset( item ) )
	RuiSetFloat( rui, "imageWidth", baseImageWidth )
	RuiSetFloat( rui, "imageHeight", baseImageHeight )
	RuiSetFloat( rui, "imageScale", baseImageScale )
	RuiSetBool( rui, "disableLoadIcon", true )
}

void function ShowBattlePassItem_Currency( ItemFlavor item, float scale )
{
	int itemType = ItemFlavor_GetType( item )
	Assert( itemType == eItemType.account_currency || itemType == eItemType.account_currency_bundle )

	asset modelAsset = $"mdl/dev/empty_model.rmdl"
	float modelScale = 1.0
	int rarity       = 0
	if ( ItemFlavor_HasQuality( item ) )
		rarity = ItemFlavor_GetQuality( item )

	if ( ItemFlavor_GetType( item ) == eItemType.account_currency )
	{
		if ( item == GRX_CURRENCIES[GRX_CURRENCY_CRAFTING] )
		{
			modelAsset = BATTLEPASS_MODEL_CRAFTING_METALS
			modelScale = 1.5
		}
		else
		{
			modelAsset = GRXCurrency_GetPreviewModel( item )
		}
	}
	else
	{
		asset itemAsset = ItemFlavor_GetAsset( item )
		Assert( itemAsset == $"settings/itemflav/currency_bundle/crafting_common.rpak" ||
		itemAsset == $"settings/itemflav/currency_bundle/crafting_rare.rpak" ||
		itemAsset == $"settings/itemflav/currency_bundle/crafting_epic.rpak" ||
		itemAsset == $"settings/itemflav/currency_bundle/crafting_legendary.rpak" ||
		itemAsset == $"settings/itemflav/currency_bundle/heirloom.rpak" )

		switch ( rarity )
		{
			case 0:
				modelAsset = CURRENCY_MODEL_COMMON
				break

			case 1:
				modelAsset = CURRENCY_MODEL_RARE
				break

			case 2:
				modelAsset = CURRENCY_MODEL_EPIC
				break

			case 3:
				modelAsset = CURRENCY_MODEL_LEGENDARY
				break

			case 4:
				ItemFlavor currencyFlav = GRXCurrencyBundle_GetCurrencyFlav( item )
				modelAsset = GRXCurrency_GetPreviewModel( currencyFlav )
				break

			default: Assert( false )
		}

		modelScale = 1.5
	}

	vector origin = fileLevel.sceneRefOrigin + <0, 0, 29>
	vector angles = fileLevel.sceneRefAngles

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamic( origin, AnglesCompose( angles, <0, 32, 0> ), modelAsset )
	model.MakeSafeForUIScriptHack()
	model.SetModelScale( scale * modelScale )
	model.SetParent( mover )

	mover.NonPhysicsRotate( <0, 0, -1>, fileLevel.rotateSpeed )

	vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
	thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )

	fileLevel.mover = mover
	fileLevel.models.append( model )
}

void function ShowBattlePassItem_XPBoost( ItemFlavor item, float scale )
{
	vector origin = fileLevel.sceneRefOrigin + <0, 0, 28.0>
	vector angles = fileLevel.sceneRefAngles

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	asset voucherAsset = Voucher_GetModel( item )

	entity model = CreateClientSidePropDynamic( origin, AnglesCompose( angles, <0, 32, 0> ), voucherAsset != $"" ? voucherAsset : BATTLEPASS_MODEL_BOOST )
	model.MakeSafeForUIScriptHack()
	model.SetParent( mover )
	model.SetModelScale( scale * 0.85 )

	mover.NonPhysicsRotate( <0, 0, -1>, fileLevel.rotateSpeed )

	vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
	thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )

	fileLevel.mover = mover
	fileLevel.models.append( model )
}

void function ShowBattlePassItem_QuestClue( ItemFlavor item, float scale )
{
	vector origin = fileLevel.sceneRefOrigin + <0, -260, 28.0>
	vector angles = fileLevel.sceneRefAngles

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamic( origin, AnglesCompose( angles, <20, 32, 0> ), $"mdl/weapons_r5/misc_pve/s5_treasure_box/w_s5_treasure_box.rmdl" )
	model.MakeSafeForUIScriptHack()
	model.SetParent( mover )
	model.SetModelScale( scale * 0.85 )

	mover.NonPhysicsRotate( <0, 0, -1>, fileLevel.rotateSpeed )

	vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
	thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )

	fileLevel.mover = mover
	fileLevel.models.append( model )
}

void function ShowBattlePassItem_CPReward( ItemFlavor item, float scale )
{
	vector origin = fileLevel.sceneRefOrigin + <0, 0, 28.0>
	vector angles = fileLevel.sceneRefAngles

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamic( origin, AnglesCompose( angles, <90, 32, 0> ), CHALLENGE_REWARD_MODEL )
	model.MakeSafeForUIScriptHack()
	model.SetParent( mover )
	model.SetModelScale( scale * 0.35 )

	mover.NonPhysicsRotate( <0, 0, -1>, fileLevel.rotateSpeed )

	if ( ItemFlavor_HasQuality( item ) )
	{
		vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
		thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )
	}

	fileLevel.mover = mover
	fileLevel.models.append( model )
}

void function ShowBattlePassItem_StarReward( ItemFlavor item, float scale )
{
	vector origin = fileLevel.sceneRefOrigin + <0, 0, 28.0>
	vector angles = fileLevel.sceneRefAngles

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamic( origin, AnglesCompose( angles, <90, 32, 0> ), BATTLEPASS_STAR_REWARD_MODEL )
	model.MakeSafeForUIScriptHack()
	model.SetParent( mover )
	model.SetModelScale( scale * 0.35 )

	mover.NonPhysicsRotate( <0, 0, -1>, fileLevel.rotateSpeed )

	if ( ItemFlavor_HasQuality( item ) )
	{
		vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
		thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )
	}

	fileLevel.mover = mover
	fileLevel.models.append( model )
}

void function ShowBattlePassItem_Voucher( ItemFlavor item, float scale )
{
	bool givesBattlepassLevels = Voucher_GetEffectBattlepassLevels( item ) > 0
	bool givesBattlepassStars = Voucher_GetEffectBattlepassStars( item ) > 0

	vector origin = fileLevel.sceneRefOrigin + <0, 0, 28.0>
	vector angles = fileLevel.sceneRefAngles

	asset voucherAsset = Voucher_GetModel( item )
	if ( voucherAsset == $"" )
	{
		if ( givesBattlepassLevels )
		{
			voucherAsset = BATTLEPASS_MODEL_BOOST
		}
		else if ( givesBattlepassStars )
		{
			voucherAsset = BATTLEPASS_STAR_REWARD_MODEL
		}
		else
		{
			return 
		}
	}

	vector angles2 = givesBattlepassStars ? <90, 32, 0> : <0, 32, 0>

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamic( origin, AnglesCompose( angles, angles2 ), voucherAsset )
	model.MakeSafeForUIScriptHack()
	model.SetParent( mover )

	if ( givesBattlepassStars )
	{
		model.SetModelScale( scale * 0.35 )
	}
	else
	{
		model.SetModelScale( scale * 0.85 )
	}

	mover.NonPhysicsRotate( <0, 0, -1>, fileLevel.rotateSpeed )

	if ( ItemFlavor_HasQuality( item ) )
	{
		vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
		thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )
	}

	fileLevel.mover = mover
	fileLevel.models.append( model )
}

const float BATTLEPASS_VIDEO_WIDTH = 600.0
const float BATTLEPASS_VIDEO_HEIGHT = 338.0

void function ShowBattlePassItem_WeaponSkinVideo( ItemFlavor item, float scale, asset video )
{
	const float BATTLEPASS_UNKNOWN_Z_OFFSET = 28

	
	vector origin = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_UNKNOWN_Z_OFFSET>
	vector angles = VectorToAngles( AnglesToForward( fileLevel.sceneRefAngles ) * -1 )

	float width  = scale * BATTLEPASS_VIDEO_WIDTH / 14.0
	float height = scale * BATTLEPASS_VIDEO_HEIGHT / 14.0

	var topo = CreateRUITopology_Worldspace( origin, angles, width, height )
	var rui  = RuiCreate( $"ui/finisher_video.rpak", topo, RUI_DRAW_VIEW_MODEL, 0 )

	fileLevel.videoChannel = ReserveVideoChannel( BattlePassVideoOnFinished )
	RuiSetInt( rui, "channel", fileLevel.videoChannel )
	StartVideoOnChannel( fileLevel.videoChannel, video, true, 0.0 )

	fileLevel.topo = topo
	fileLevel.rui = rui
}


void function ShowBattlePassItem_MusicPack( ItemFlavor item, float scale, bool shouldPlayAudioPreview )
{
	int itemType = ItemFlavor_GetType( item )
	Assert( itemType == eItemType.music_pack )

	const float BATTLEPASS_QUIP_WIDTH = 390.0
	const float BATTLEPASS_QUIP_HEIGHT = 208.0
	const float BATTLEPASS_QUIP_SCALE = 0.091
	const float BATTLEPASS_QUIP_Z_OFFSET = 20.5
	const asset BATTLEPASS_QUIP_BG_MODEL = $"mdl/menu/loot_ceremony_quip_bg.rmdl"

	vector origin        = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_QUIP_Z_OFFSET>
	vector angles        = fileLevel.sceneRefAngles
	vector placardAngles = VectorToAngles( AnglesToForward( angles ) * -1 )

	
	float width  = scale * BATTLEPASS_QUIP_WIDTH * BATTLEPASS_QUIP_SCALE
	float height = scale * BATTLEPASS_QUIP_HEIGHT * BATTLEPASS_QUIP_SCALE

	entity model = CreateClientSidePropDynamic( origin, angles, BATTLEPASS_QUIP_BG_MODEL )
	model.MakeSafeForUIScriptHack()
	model.SetModelScale( scale * BATTLEPASS_QUIP_SCALE )

	var topo = CreateRUITopology_Worldspace( origin + <0, 0, (height * 0.5)>, placardAngles, width, height )
	var rui  = RuiCreate( $"ui/loot_reward_intro_quip.rpak", topo, RUI_DRAW_WORLD, 0 )

	string previewAlias = MusicPack_GetPreviewMusic( item )

	RuiSetBool( rui, "isVisible", true )
	RuiSetBool( rui, "battlepass", true )
	RuiSetInt( rui, "rarity", ItemFlavor_GetQuality( item ) )
	RuiSetImage( rui, "portraitImage", MusicPack_GetPortraitImage( item ) )
	RuiSetFloat( rui, "portraitBlend", MusicPack_GetPortraitBlend( item ) )
	RuiSetString( rui, "quipTypeText", "#MUSIC_PACK" )
	RuiTrackFloat( rui, "level", null, RUI_TRACK_SOUND_METER, 0 )

	fileLevel.models.append( model )
	fileLevel.topo = topo
	fileLevel.rui = rui

	
	if ( previewAlias != "" && shouldPlayAudioPreview )
	{
		fileLevel.playingPreviewAlias = previewAlias
		EmitSoundOnEntity( GetLocalClientPlayer(), previewAlias )
	}
}


void function ShowBattlePassItem_Loadscreen( ItemFlavor item, float scale )
{
	if ( fileLevel.loadscreenPreviewBox != null && IsValid( fileLevel.loadscreenPreviewBox ) )
	{
		UpdateLoadscreenPreviewMaterial( fileLevel.loadscreenPreviewBox, null, ItemFlavor_GetGUID( item ) )
	}
}

void function ShowBattlePassItem_RadioPlay( ItemFlavor item, float scale )
{
	const float BATTLEPASS_RADIO_PLAY_WIDTH = 440
	const float BATTLEPASS_RADIO_PLAY_HEIGHT = 248
	const float BATTLEPASS_RADIO_PLAY_Z_OFFSET = 30

	
	vector origin = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_RADIO_PLAY_Z_OFFSET>
	vector angles = VectorToAngles( AnglesToForward( fileLevel.sceneRefAngles ) * -1 )

	float width  = scale * BATTLEPASS_RADIO_PLAY_WIDTH
	float height = scale * BATTLEPASS_RADIO_PLAY_HEIGHT

	var topo = CreateRUITopology_Worldspace( origin, angles, width, height )
	var rui  = RuiCreate( $"ui/world_space_radio_play.rpak", topo, RUI_DRAW_VIEW_MODEL, 0 )

	RuiSetImage( rui, "radioPlayImage", ItemFlavor_GetIcon( item ) )

	fileLevel.topo = topo
	fileLevel.rui = rui
}




























void function ShowBattlePassItem_SkydiveEmote( ItemFlavor item, float scale )
{
	const float BATTLEPASS_UNKNOWN_Z_OFFSET = 28

	
	vector origin = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_UNKNOWN_Z_OFFSET>
	vector angles = VectorToAngles( AnglesToForward( fileLevel.sceneRefAngles ) * -1 )

	float width  = scale * BATTLEPASS_VIDEO_WIDTH / 14.0
	float height = scale * BATTLEPASS_VIDEO_HEIGHT / 14.0

	var topo = CreateRUITopology_Worldspace( origin, angles, width, height )
	var rui  = RuiCreate( $"ui/finisher_video.rpak", topo, RUI_DRAW_VIEW_MODEL, 0 )

	fileLevel.videoChannel = ReserveVideoChannel( BattlePassVideoOnFinished )
	RuiSetInt( rui, "channel", fileLevel.videoChannel )
	StartVideoOnChannel( fileLevel.videoChannel, SkydiveEmote_GetVideo( item ), true, 0.0 )

	fileLevel.topo = topo
	fileLevel.rui = rui
}


void function ShowBattlePassItem_QuestComicPage( ItemFlavor item, float scale )
{
	Assert( ItemFlavor_GetType( item ) == eItemType.quest_comic )

	const float BATTLEPASS_UNKNOWN_Z_OFFSET = 25

	
	vector origin = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_UNKNOWN_Z_OFFSET>
	vector angles = VectorToAngles( AnglesToForward( fileLevel.sceneRefAngles ) * -1 )

	float width  = scale * BATTLEPASS_VIDEO_WIDTH / 16.0
	float height = scale * BATTLEPASS_VIDEO_HEIGHT / 16.0

	var topo = CreateRUITopology_Worldspace( origin, angles, width, height )
	var rui  = RuiCreate( $"ui/quest_reward_mission.rpak", topo, RUI_DRAW_VIEW_MODEL, 0 )

	var settingsBlock      = ItemFlavor_GetSettingsBlock( item )
	string comicPageName   = ItemFlavor_GetShortName( item )
	string comicPageDesc   = ItemFlavor_GetShortDescription( item )
	asset comicRewardImage = Comic_GetPreviewImage( item )

	RuiSetString( rui, "missionName", comicPageName )
	RuiSetString( rui, "missionDesc", comicPageDesc )
	RuiSetImage( rui, "missionImage", comicRewardImage )

	fileLevel.topo = topo
	fileLevel.rui = rui
}


void function ShowBattlePassItem_QuestMission( ItemFlavor item, float scale )
{
	Assert( ItemFlavor_GetType( item ) == eItemType.quest_mission )

	const float BATTLEPASS_UNKNOWN_Z_OFFSET = 25

	
	vector origin = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_UNKNOWN_Z_OFFSET>
	vector angles = VectorToAngles( AnglesToForward( fileLevel.sceneRefAngles ) * -1 )

	float width  = scale * BATTLEPASS_VIDEO_WIDTH / 16.0
	float height = scale * BATTLEPASS_VIDEO_HEIGHT / 16.0

	var topo = CreateRUITopology_Worldspace( origin, angles, width, height )
	var rui  = RuiCreate( $"ui/quest_reward_mission.rpak", topo, RUI_DRAW_VIEW_MODEL, 0 )

	var settingsBlock   = ItemFlavor_GetSettingsBlock( item )
	string playlistName = GetSettingsBlockString( settingsBlock, "playlistName" )
	string missionName  = GetPlaylistVarString( playlistName, "name", "#PLAYLIST_UNAVAILABLE" )
	string missionDesc  = GetPlaylistVarString( playlistName, "description", "#HUD_UNKNOWN" )
	string imageKey     = GetPlaylistVarString( playlistName, "image", "" )
	asset missionImage  = GetImageFromImageMap( imageKey )

	RuiSetString( rui, "missionName", missionName )
	RuiSetString( rui, "missionDesc", missionDesc )
	RuiSetImage( rui, "missionImage", missionImage )

	fileLevel.topo = topo
	fileLevel.rui = rui
}


void function ShowBattlePassItem_Level( ItemFlavor item, float scale )
{
	vector origin = fileLevel.sceneRefOrigin + <0, 0, 28.0>
	vector angles = fileLevel.sceneRefAngles
	const asset BATTLEPASS_LEVEL_MODEL = $"mdl/menu/bp_badge.rmdl"

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamic( origin, AnglesCompose( angles, <0, 32, 0> ), BATTLEPASS_LEVEL_MODEL )
	model.MakeSafeForUIScriptHack()
	model.SetParent( mover )
	model.SetModelScale( scale * 0.85 )
	mover.NonPhysicsRotate( <0, 0, -1>, fileLevel.rotateSpeed )

	fileLevel.mover = mover
	fileLevel.models.append( model )
}


void function ShowBattlePassItem_Character( ItemFlavor item, float scale )
{
	ItemFlavor char = item
	ItemFlavor characterSkin = CharacterClass_GetDefaultSkin( char )
	vector origin   = fileLevel.sceneRefOrigin + <0, 0, 4.0> - 0.6 * CharacterClass_GetMenuZoomOffset( char )
	vector angles   = fileLevel.sceneRefAngles

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamic( origin, angles, $"mdl/dev/empty_model.rmdl" )

	
	if ( fileLevel.sceneRefName == "battlepass_center_ref" )
		model.SetLightingOrigin( <11328, 2612, 28> ) 

	CharacterSkin_Apply( model, characterSkin )
	model.MakeSafeForUIScriptHack()
	model.SetModelScale( scale * 0.75 )
	model.SetParent( mover )
	
	thread MoverPendulum( mover )

	thread PlayAnim( model, "ACT_MP_MENU_LOOT_CEREMONY_IDLE", mover )

	vector flashColor = ItemFlavor_GetQualityColor( characterSkin ) / 255
	thread FlashMenuModel( model, eMenuModelFlashType.BATTLEPASS, flashColor )

	fileLevel.mover = mover
	fileLevel.models.append( model )
}


void function ShowBattlePassItem_Sticker( ItemFlavor item )
{
	vector origin = fileLevel.sceneRefOrigin + <0, 0, 28>
	vector angles = fileLevel.sceneRefAngles

	entity mover = CreateClientsideScriptMover( $"mdl/dev/empty_model.rmdl", origin, angles )
	mover.MakeSafeForUIScriptHack()

	entity model = CreateClientSidePropDynamic( origin, angles, UNAPPLIED_STICKER_MODEL )
	model.MakeSafeForUIScriptHack()
	model.SetModelScale( 1.5 )
	model.SetParent( mover )

	asset stickerMat = Sticker_GetReplacementMaterialAsset( item )
	int stickerInstance = Sticker_SetMaterialModForLocalPlayer( model, stickerMat )

	vector flashColor = ItemFlavor_GetQualityColor( item ) / 255
	Sticker_CreateFlashData( stickerInstance, model, eMenuModelFlashType.BATTLEPASS, flashColor )
	Sticker_OnPlaced( stickerInstance, Sticker_FlashOnLoadComplete )

	fileLevel.mover = mover
	fileLevel.models.append( model )
}


void function ShowBattlePassItem_ArtifactBink( ItemFlavor item, float scale )
{
	const float BATTLEPASS_UNKNOWN_Z_OFFSET = 28

	
	vector origin = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_UNKNOWN_Z_OFFSET>
	vector angles = VectorToAngles( AnglesToForward( fileLevel.sceneRefAngles ) * -1 )

	float width  = scale * BATTLEPASS_VIDEO_WIDTH / 14.0
	float height = scale * BATTLEPASS_VIDEO_HEIGHT / 14.0

	var topo = CreateRUITopology_Worldspace( origin, angles, width, height )
	var rui  = RuiCreate( $"ui/finisher_video.rpak", topo, RUI_DRAW_VIEW_MODEL, 0 )

	fileLevel.videoChannel = ReserveVideoChannel( BattlePassVideoOnFinished )
	RuiSetInt( rui, "channel", fileLevel.videoChannel )

	asset video
	switch( ItemFlavor_GetType( item ) )
	{
		case eItemType.artifact_component_deathbox:
			video = Deathbox_GetVideo( item )
			break

		case eItemType.artifact_component_activation_emote:
			video = Artifacts_ActivationEmote_GetVideo( item )
			break

		default:
			Assert( false )
			return
	}

	StartVideoOnChannel( fileLevel.videoChannel, video, true, 0.0 )

	fileLevel.topo = topo
	fileLevel.rui = rui
}



void function ShowBattlePassItem_Unknown( ItemFlavor item, float scale )
{
	const float BATTLEPASS_UNKNOWN_Z_OFFSET = 25

	
	vector origin = fileLevel.sceneRefOrigin + <0, 0, BATTLEPASS_UNKNOWN_Z_OFFSET>
	vector angles = VectorToAngles( AnglesToForward( fileLevel.sceneRefAngles ) * -1 )

	float width  = scale * BATTLEPASS_VIDEO_WIDTH / 16.0
	float height = scale * BATTLEPASS_VIDEO_HEIGHT / 16.0

	var topo = CreateRUITopology_Worldspace( origin, angles, width, height )
	var rui  = RuiCreate( $"ui/loot_reward_temp.rpak", topo, RUI_DRAW_WORLD, 0 )

	RuiSetString( rui, "bodyText", Localize( ItemFlavor_GetLongName( item ) ) )

	fileLevel.topo = topo
	fileLevel.rui = rui
}


void function BattlePassVideoOnFinished( int channel )
{
}





















void function InitBattlePassLights()
{
	fileLevel.stationaryLights = GetEntArrayByScriptName( "battlepass_stationary_light" )
	fileLevel.stationaryLightOffsets.clear()

	entity ref = GetEntByScriptName( "battlepass_right_ref" )

	foreach ( light in fileLevel.stationaryLights )
	{
		fileLevel.stationaryLightOffsets[ light ] <- light.GetOrigin() - ref.GetOrigin()
	}

	
	





}


void function BattlePassLightsOn()
{
	foreach ( light in fileLevel.stationaryLights )
		light.SetTweakLightUpdateShadowsEveryFrame( true )

	

	









































}

void function BattlePassLightsOff()
{
	foreach ( light in fileLevel.stationaryLights )
		light.SetTweakLightUpdateShadowsEveryFrame( false )

	

	








}

































string function BattlePass_PopulateAboutTitle()
{
	return "#BATTLEPASS_BREAKOUT_TITLE"
}



























string function Season_GetLongName( ItemFlavor season )
{
	return ItemFlavor_GetLongName( season )
}


string function Season_GetShortName( ItemFlavor season )
{
	return Localize( ItemFlavor_GetShortName( season ) )
}


string function Season_GetTimeRemainingText( ItemFlavor season )
{
	int seasonEndUnixTime   = CalEvent_GetFinishUnixTime( season )
	int remainingSeasonTime = seasonEndUnixTime - GetUnixTimestamp()

	if ( remainingSeasonTime <= 0 )
		return Localize( "#BATTLE_PASS_SEASON_ENDED" )

	DisplayTime dt = SecondsToDHMS( remainingSeasonTime )

	return Localize( "#SEASON_ENDS_IN_DAYS", string( dt.days ) )
}


asset function Season_GetSmallLogo( ItemFlavor season )
{
	ItemFlavor pass = Season_GetBattlePass( season )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( pass ), "smallLogo" )
}


asset function Season_GetLobbyBannerLeftImage( ItemFlavor season )
{
	ItemFlavor pass = Season_GetBattlePass( season )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( season ), "lobbyButtonImage" )
}

asset function Season_GetLobbyBannerRightImage( ItemFlavor season )
{
	ItemFlavor pass = Season_GetBattlePass( season )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( season ), "lobbyBgRightImage" )
}

asset function Season_GetSmallLogoBg( ItemFlavor season )
{
	ItemFlavor pass = Season_GetBattlePass( season )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( season ), "lobbyDiamondImage" )
}


vector function Season_GetTitleTextColor( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "seasonTitleColor" )
}


vector function Season_GetHeaderTextColor( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "seasonHeaderColor" )
}


vector function Season_GetTimeRemainingTextColor( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "seasonTimeRemainingColor" )
}

vector function Season_GetNewColor( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "seasonNewColor" )
}

vector function Season_GetColor( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "seasonCol" )
}


vector function Season_GetTabBGFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabBGFocusedCol" )
}

vector function Season_GetTabBarFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabBarFocusedCol" )
}

vector function Season_GetTabBGSelectedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabBGSelectedCol" )
}

vector function Season_GetTabBarSelectedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabBarSelectedCol" )
}

vector function Season_GetTabTextDefaultCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabTextDefaultCol" )
}

vector function Season_GetTabTextFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabTextFocusedCol" )
}

vector function Season_GetTabTextSelectedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabTextSelectedCol" )
}

vector function Season_GetTabGlowFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "tabGlowFocusedCol" )
}


vector function Season_GetSubTabBGFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "subtabBGFocusedCol" )
}

vector function Season_GetSubTabBarFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "subtabBarFocusedCol" )
}

vector function Season_GetSubTabBGSelectedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "subtabBGSelectedCol" )
}

vector function Season_GetSubTabBarSelectedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "subtabBarSelectedCol" )
}

vector function Season_GetSubTabTextDefaultCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "subtabTextDefaultCol" )
}

vector function Season_GetSubTabTextFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "subtabTextFocusedCol" )
}

vector function Season_GetSubTabTextSelectedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "subtabTextSelectedCol" )
}

vector function Season_GetSubTabGlowFocusedCol( ItemFlavor event )
{
	Assert( ItemFlavor_GetType( event ) == eItemType.calevent_season )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( event ), "subtabGlowFocusedCol" )
}


#if DEV
void function DEV_TweakMover( vector origin )
{
	fileLevel.mover.SetOrigin( origin )
}
#endif
