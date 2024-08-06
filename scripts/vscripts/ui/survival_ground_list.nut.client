


global function SurvivalGroundList_LevelInit













global function IsGroundListMenuOpen
global function UpdateSurvivalGroundList

global function UIToClient_SurvivalGroundListOpened
global function UIToClient_SurvivalGroundListClosed
global function UIToClient_SurvivalGroundList_UpdateQuickSwapItem
global function UIToClient_SurvivalGroundList_OnQuickSwapItemClick
global function UIToClient_SurvivalGroundList_OnMouseLeftPressed
global function UIToClient_RestrictedLootConfirmDialog_DoIt

global function UIToClient_CloseQuickSwapIfOpen

global function ServerToClient_UpdateItem



global struct SurvivalGroundListUpdateParams
{
	entity        player
	array<entity> currentLootEnts
	bool          isBlackMarket
	int           blackMarketUseCount
}




struct DeathBoxEntryData
{
	string        key
	LootData&     lootFlav
	array<entity> lootEnts 
	bool          isClickable
	bool          isUsable
	bool          isRelevant
	bool          isUpgrade
	bool          isBlocked
	int           pingCounter
	int           useCounter
	bool		  isRestrictedItem
	int			  restrictedType

	int            totalCount = 0
	int            lastSeenTotalCount = 0
	EncodedEHandle lastSeenBestLootEntEEH = EncodedEHandle_null

	bool wasLastLootPickedUpByLocalPlayer = false
	bool predictingNewEntToBeCreated = false
}




struct PredictedLootActionData
{
	int   type
	float time

	
	string lootFlavRef
	int    count 

	
	EncodedEHandle bestLootEntEEH

	int        expectedInventoryContribution = 0
	int ornull backpackSwapSlot = null

	bool actionIsFromVendingMachine = false

#if DEV
		bool devMsgPrinted = false
#endif
}




struct FileStruct_LifetimeLevel
{
	table signalDummy


		var menu
		var holdToUseElem

		var backer
		var listPanel
		var scrollBar

		var quickSwapBacker
		var quickSwapGrid
		var quickSwapHeader
		var inventorySwapIcon

		var blackMarketWidget

		EncodedEHandle                         currentDeathBoxEEH
		table<string, DeathBoxEntryData>       deathBoxEntryDataByKey
		table<entity, DeathBoxEntryData>       deathBoxEntryDataByLootEnt
		DeathBoxEntryData ornull               currentQuickSwapEntry
		var                                    currentQuickSwapItemButton
		bool                                   currentQuickSwapIsAltAction
		string                                 specialStateSamenessKey
		array<PredictedLootActionData>         predictedActions
		bool                                   predictedActionsDirty = false

		bool								   insideNormalRange = false


		vector categoryHeaderTextCol

}


FileStruct_LifetimeLevel fileLevel 















void function SurvivalGroundList_LevelInit()
{






		RegisterSignal( "SurvivalGroundList_Closed" )
		RegisterSignal( "DeathBoxEntryData_Update" )
		RegisterSignal( "DeathBoxEntryData_Unbind" )
		AddCallback_OnPingCreatedByAnyPlayer( OnPingCreatedByAnyPlayer )
		AddLocalPlayerTookDamageCallback( OnLocalPlayerDamaged )

		if ( !IsLobby() )
		{
			foreach ( equipSlot, data in EquipmentSlot_GetAllEquipmentSlots() )
			{
				if ( data.trackingNetInt == "" )
					continue
				AddCallback_OnEquipSlotTrackingIntChanged( equipSlot, OnPlayerEquipmentChanged )
			}
		}

}





































































































void function UIToClient_SurvivalGroundListOpened( var menu )
{
	CloseQuickSwapIfOpen()

	if ( !IsValid( fileLevel.menu ) )
	{
		fileLevel.menu = menu
		fileLevel.holdToUseElem = Hud_GetChild( menu, "HoldToUseElem" )

		fileLevel.backer = Hud_GetChild( menu, "Backer" )
		fileLevel.listPanel = Hud_GetChild( menu, "ListPanel" )
		fileLevel.scrollBar = Hud_GetChild( menu, "ScrollBar" )

		fileLevel.quickSwapBacker = Hud_GetChild( menu, "QuickSwapBacker" )
		fileLevel.inventorySwapIcon = Hud_GetChild( menu, "SwapIcon" )
		fileLevel.quickSwapHeader = Hud_GetChild( menu, "QuickSwapHeader" )
		fileLevel.quickSwapGrid = Hud_GetChild( menu, "QuickSwapGrid" )

		fileLevel.blackMarketWidget = Hud_GetChild( menu, "BlackMarketWidget" )

		DeathBoxListPanelData data = InitDeathBoxListPanel( fileLevel.listPanel, fileLevel.scrollBar, ItemButtonSetup )
		data.edgePadding = 17
		data.maxRowWidth = 406 + 110
		data.sortAscending = false
		data.anchorCategoryHeadersToTop = true
		data.categoryHeaderBindCallback = OnCategoryHeaderBind
		data.categoryHeaderUnbindCallback = OnCategoryHeaderUnbind
		data.itemBindCallback = OnItemBind
		data.itemUnbindCallback = OnItemUnbind
		data.itemClickCallback = OnItemClick
		data.itemClickRightCallback = OnItemClickRight
		data.itemFocusGetCallback = OnItemFocusGet
		data.itemFocusLoseCallback = OnItemFocusLose
		data.itemKeyEventHandler = OnItemKeyEvent
		data.itemCommandEventHandler = OnItemCommandEvent
	}

	entity deathBox = Survival_GetDeathBox()

	if ( !IsValid( deathBox ) )
	{
		RunUIScript( "CloseAllMenus" )
		return
	}
	fileLevel.currentDeathBoxEEH = deathBox.GetEncodedEHandle()
	fileLevel.deathBoxEntryDataByKey.clear()
	fileLevel.deathBoxEntryDataByLootEnt.clear()
	fileLevel.specialStateSamenessKey = ""


	fileLevel.categoryHeaderTextCol = SrgbToLinear( <240, 240, 240> / 255.0 * 0.85 )
	DeathBoxListPanel_SetScrollBarColorAlpha( fileLevel.listPanel, <1.0, 1.0, 1.0>, 1.0 )

	bool isBlackMarket = (deathBox.GetNetworkedClassName() == "prop_loot_grabber")

		bool isVendingMachine = false



		if ( isBlackMarket )
		{
			if ( IsVendingMachineUnsafe( deathBox ) )
			{
				isVendingMachine = true
				isBlackMarket = false
			}







		}

	if ( isBlackMarket )
	{
		EmitUISound( "Loba_Ultimate_BlackMarket_Open" )
		fileLevel.categoryHeaderTextCol = SrgbToLinear( <255, 255, 255> / 255.0 )
		DeathBoxListPanel_SetScrollBarColorAlpha( fileLevel.listPanel, <218.0 / 255.0, 235.0 / 255.0, 245.0 / 255.0>, 1.0 )
	}

	DeathBoxListPanel_Reset( fileLevel.listPanel )
	DeathBoxListPanel_SetActive( fileLevel.listPanel, true )

	foreach ( string enumKey, int enumVal in eLootSortCategories )
	{
		DeathBoxListPanelCategory category
		category.key = format( "%02d", 99 - enumVal )
		category.sortOrdinal = category.key
		category.headerLabel = GetCategoryTitleFromPriority( enumVal )
		category.headerHeight = 30
		if ( enumVal == eLootSortCategories.WEAPONS )
		{
			category.itemWidth = 200 + 55
			category.itemHeight = 100
			category.itemPadding = 6
		}
		else if ( enumVal == eLootSortCategories.AMMO )
		{
			category.itemWidth = 100
			category.itemHeight = 100
			category.itemPadding = 4
		}

		else if ( enumVal == eLootSortCategories.HEALING )
		{
			category.itemWidth   = 100
			category.itemHeight  = 100
			category.itemPadding = 4
		}

		else
		{
			category.itemWidth = 406 + 110
			category.itemHeight = 68
			category.itemPadding = 6
		}
		category.bottomPadding = 27
		DeathBoxListPanel_AddCategory( fileLevel.listPanel, category )
	}

	
	foreach ( string ammoPoolTypeKey, int ammoPoolTypeVal in eAmmoPoolType )
	{
		if ( !ArrowsCanBePickedUp() && ammoPoolTypeVal == eAmmoPoolType.arrows )
			continue

		DeathBoxEntryData entryData
		entryData.lootFlav = SURVIVAL_Loot_GetLootDataByRef( ammoPoolTypeKey )
		entryData.key = entryData.lootFlav.ref
		fileLevel.deathBoxEntryDataByKey[entryData.key] <- entryData
		entryData.lootEnts = []

		DeathBoxListPanelItem item
		item.categoryKey = format( "%02d", 99 - eLootSortCategories.AMMO )
		item.key = entryData.key
		item.sortOrdinal = format( "%02d", 50 - ammoPoolTypeVal )
		DeathBoxListPanel_AddItem( fileLevel.listPanel, item )
	}


	array<string> healingTypes = [ "health_pickup_health_small", "health_pickup_combo_small", "health_pickup_health_large", "health_pickup_combo_large", "health_pickup_combo_full" ]
	
	foreach ( string healingItemType in healingTypes )
	{
		DeathBoxEntryData entryData
		entryData.lootFlav = SURVIVAL_Loot_GetLootDataByRef( healingItemType )
		entryData.key = entryData.lootFlav.ref
		fileLevel.deathBoxEntryDataByKey[entryData.key] <- entryData
		entryData.lootEnts = []

		DeathBoxListPanelItem item
		item.categoryKey = format( "%02d", 99 - eLootSortCategories.HEALING )
		item.key = entryData.key
		DeathBoxListPanel_AddItem( fileLevel.listPanel, item )
	}


	thread Delayed_SetCursorToObject( fileLevel.backer )

	string headerMainText       = Localize( "#DEATHBOX_MENU_HEADER" )
	string headerOwnerText      = ""
	string headerRightText      = ""
	asset headerImage           = $""
	asset headerBackgroundImage = $""
	float headerBackgroundImageAlpha = 0.12
	vector headerBackgroundImageSize = <162, 91, 0>

	if ( isBlackMarket )
	{
		headerMainText = Localize( "#BLACK_MARKET_MENU_HEADER" )
		headerRightText = Localize( "#NEARBY_ITEMS" )

		entity deathBoxOwner = deathBox.GetOwner()
		if ( IsValid( deathBoxOwner ) && deathBoxOwner.IsPlayer() )
		{
			headerOwnerText = Localize( "#DEATHBOX_OWNER_SUFFIX", deathBoxOwner.GetPlayerName() )
		}
	}

	else if ( isVendingMachine )
	{
		headerMainText = Localize( "#VENDING_MACHINE_MENU_HEADER" )
		headerRightText = Localize( "#VENDING_MACHINE_RIGHT_HEADER" )
		headerBackgroundImage = $"rui/hud/common/header_loot_crate"
		headerBackgroundImageAlpha = 1.0
		headerBackgroundImageSize = <1600, 175, 0>
	}








	else
	{
		string customOwnerName = deathBox.GetCustomOwnerName()
		if ( customOwnerName != "" )
		{
			headerOwnerText = Localize( "#DEATHBOX_OWNER_SUFFIX", customOwnerName )
			int characterIdx = deathBox.GetNetInt( "characterIndex" )
			if ( IsValidLoadoutSlotContentsIndexForItemFlavor( Loadout_Character(), characterIdx ) )
			{
				ItemFlavor char = ConvertLoadoutSlotContentsIndexToItemFlavor( Loadout_Character(), characterIdx )
				headerImage = CharacterClass_GetGalleryPortrait( char )
				headerBackgroundImage = CharacterClass_GetGalleryPortraitBackground( char )
			}
		}
		else
		{
			EHI ornull ownerEHI = GetDeathBoxOwnerEHI( deathBox )
			if ( ownerEHI != null )
			{
				expect EHI( ownerEHI )
				if ( EHIHasValidScriptStruct( ownerEHI ) )
				{
					headerOwnerText = Localize( "#DEATHBOX_OWNER_SUFFIX", GetDisplayablePlayerNameFromEHI( ownerEHI ) )
					if ( LoadoutSlot_IsReady( ownerEHI, Loadout_Character() ) )
					{
						ItemFlavor char = LoadoutSlot_GetItemFlavor( ownerEHI, Loadout_Character() )
						headerImage = CharacterClass_GetGalleryPortrait( char )
						headerBackgroundImage = CharacterClass_GetGalleryPortraitBackground( char )
					}
				}
			}
		}
	}

	
	HudElem_SetRuiArg( fileLevel.backer, "headerMainText", headerMainText )
	HudElem_SetRuiArg( fileLevel.backer, "headerOwnerText", headerOwnerText )
	HudElem_SetRuiArg( fileLevel.backer, "headerRightText", headerRightText )
	HudElem_SetRuiArg( fileLevel.backer, "headerImage", headerImage )
	HudElem_SetRuiArg( fileLevel.backer, "headerBackgroundImage", headerBackgroundImage )
	HudElem_SetRuiArg( fileLevel.backer, "headerBackgroundImageAlpha", headerBackgroundImageAlpha )
	HudElem_SetRuiArg( fileLevel.backer, "headerBackgroundImageSize", headerBackgroundImageSize )
	HudElem_SetRuiArg( fileLevel.backer, "isBlackMarket", isBlackMarket )
	HudElem_SetRuiArg( fileLevel.backer, "resetAlterAnim", true )


	if ( deathBox.GetNetworkedClassName() == "prop_death_box" )
	{
		thread CheckForDeathboxBreachedByAlterChanged_Thread( deathBox )
		RemoteDeathboxInteractOnOpenDeathbox( deathBox )
	}
	else
	{
		HudElem_SetRuiArg( fileLevel.backer, "isBreachedByAlter", false )
	}


	UIToClient_GroundlistOpened()

	if ( DoesPlayerHaveWeaponSling( GetLocalViewPlayer() ) )
		SlingSetIsGroundListMenuOpen()

	if ( isBlackMarket )
	{
		BlackMarket_OnDeathBoxMenuOpened( deathBox )
	}

	WeaponStatusSetDeathBoxMenuOpen( true )
}




void function CheckForDeathboxBreachedByAlterChanged_Thread( entity deathbox )
{
	Assert( IsValid( deathbox ) )

	EndSignal( fileLevel.signalDummy, "SurvivalGroundList_Closed" )
	EndSignal( deathbox, "OnDestroy" )

	while( true )
	{
		if ( fileLevel.backer == null )
			return

		HudElem_SetRuiArg( fileLevel.backer, "isBreachedByAlter", IsDeathboxAccessedByEnemyAlter( deathbox ) )

		WaitFrame()
	}
}




void function UIToClient_SurvivalGroundListClosed()
{
	Signal( fileLevel.signalDummy, "SurvivalGroundList_Closed" )

	entity deathBox = Survival_GetDeathBox()
	if ( IsValid( deathBox ) && deathBox.GetNetworkedClassName() == "prop_loot_grabber" )
	{

		if ( IsVendingMachineUnsafe( deathBox ) )
		{
			
		}






		else

		{
			EmitUISound( "Loba_Ultimate_BlackMarket_Close" )
		}
	}

	fileLevel.currentDeathBoxEEH = EncodedEHandle_null
	DeathBoxListPanel_SetActive( fileLevel.listPanel, false )
	UIToClient_GroundlistClosed()
	HudElem_SetRuiArg( fileLevel.backer, "resetAlterAnim", false )

	if ( DoesPlayerHaveWeaponSling( GetLocalViewPlayer() ) )
		SlingSetIsGroundListMenuOpen()

	WeaponStatusSetDeathBoxMenuOpen( false )
}




void function OnPlayerEquipmentChanged( entity player, string equipSlot, int idx )
{
	if ( player != GetLocalViewPlayer() )
		return

	fileLevel.specialStateSamenessKey = "haha ur equipment changed" 
}




void function UpdateSurvivalGroundList( SurvivalGroundListUpdateParams params )
{
	entity deathBox = GetEntityFromEncodedEHandle( fileLevel.currentDeathBoxEEH )

	bool outOfRange = !IsPlayerCloseEnoughToDeathBoxToLoot(params.player, deathBox)


		bool needToRefreshItemsDueToRange = false


	if ( !IsValid( deathBox ) || outOfRange || GetGameState() >= eGameState.WinnerDetermined || GetLocalViewPlayer().ContextAction_IsReviving() )
	{

		fileLevel.insideNormalRange = false

		RunUIScript( "CloseAllMenus" )
		return
	}


	if ( DistanceSqr( params.player.GetOrigin(), deathBox.GetOrigin() ) < ( DEATH_BOX_MAX_DIST * DEATH_BOX_MAX_DIST ) )
	{
		if ( !fileLevel.insideNormalRange )
		{
			needToRefreshItemsDueToRange = true
		}

		fileLevel.insideNormalRange = true
		HudElem_SetRuiArg( fileLevel.backer, "isAlterPassive", false )
	}
	else
	{
		
		if ( fileLevel.insideNormalRange && fileLevel.deathBoxEntryDataByLootEnt.len() != 0 )
		{
			RunUIScript( "CloseAllMenus" )
			return
		}
		fileLevel.insideNormalRange = false
		HudElem_SetRuiArg( fileLevel.backer, "isAlterPassive", true )
	}


	float maxPredictedPickupTimeBeforeAssumingMispredict = GetCurrentPlaylistVarFloat( "death_box_menu_max_predicted_pickup_time", 0.55 )
	for ( int predictedPickupIdx = 0; predictedPickupIdx < fileLevel.predictedActions.len(); predictedPickupIdx++ )
	{
		PredictedLootActionData plad = fileLevel.predictedActions[predictedPickupIdx]

		if ( Time() > plad.time + maxPredictedPickupTimeBeforeAssumingMispredict )
		{
			entity ent = GetEntityFromEncodedEHandle( plad.bestLootEntEEH )
			Warning( "Mispredicted loot action for %s x%d (%s)", plad.lootFlavRef, plad.count, IsValid( ent ) ? string(ent) : ("invalid: " + plad.bestLootEntEEH) )
			fileLevel.predictedActions.remove( predictedPickupIdx )
			predictedPickupIdx-- 
			fileLevel.predictedActionsDirty = true
			RefreshQuickSwapIfOpen()
		}

#if DEV
			if ( !plad.devMsgPrinted )
			{
				plad.devMsgPrinted = true
				entity ent = GetEntityFromEncodedEHandle( plad.bestLootEntEEH )
				printf( "Death box loot action prediction: %s, %s x%d (%s)", DEV_GetEnumStringSafe( "eLootAction", plad.type ), plad.lootFlavRef, plad.count, IsValid( ent ) ? string(ent) : ("invalid: " + plad.bestLootEntEEH) )
			}
#endif
	}

	table<entity, DeathBoxEntryData> previousDeathBoxEntryDataByLootEnt = clone fileLevel.deathBoxEntryDataByLootEnt

	table<DeathBoxEntryData, void> entriesToUpdateSet = {}

	
	foreach ( entity lootEnt in params.currentLootEnts )
	{
		Assert( IsValid( lootEnt ) )
		Assert( lootEnt.GetNetworkedClassName() == "prop_survival" )
		if ( !IsValid( lootEnt ) || lootEnt.GetNetworkedClassName() != "prop_survival" )
			continue

		
		if ( lootEnt in fileLevel.deathBoxEntryDataByLootEnt )
		{
			Assert( lootEnt in previousDeathBoxEntryDataByLootEnt )
			delete previousDeathBoxEntryDataByLootEnt[lootEnt] 

			if ( lootEnt.GetClipCount() != lootEnt.e.deathBoxMenu_lastSeenClipCount )
			{
				lootEnt.e.deathBoxMenu_lastSeenClipCount = lootEnt.GetClipCount()
				DeathBoxEntryData entryData = fileLevel.deathBoxEntryDataByLootEnt[lootEnt]
				entriesToUpdateSet[entryData] <- IN_SET
			}

			if ( needToRefreshItemsDueToRange )
			{
				LootData lootFlavor = SURVIVAL_Loot_GetLootDataByIndex( lootEnt.GetSurvivalInt() )
				if (lootFlavor.lootType == eLootType.ARMOR)
				{
					DeathBoxEntryData entryData = fileLevel.deathBoxEntryDataByLootEnt[lootEnt]
					entriesToUpdateSet[entryData] <- IN_SET
				}
			}

			continue
		}

		

		DeathBoxEntryData entryData

		LootData lootFlavor = SURVIVAL_Loot_GetLootDataByIndex( lootEnt.GetSurvivalInt() )
		bool isArmor        = (lootFlavor.lootType == eLootType.ARMOR)
		bool hasSpecialAmmo = (lootFlavor.lootType == eLootType.MAINWEAPON && !GetWeaponInfoFileKeyField_GlobalBool( lootFlavor.baseWeapon, "uses_ammo_pool" ))

		string itemKey = (hasSpecialAmmo ? format( "specialammo%d", lootEnt.GetEncodedEHandle() ) : lootFlavor.ref)
		if ( itemKey in fileLevel.deathBoxEntryDataByKey )
		{
			
			entryData = fileLevel.deathBoxEntryDataByKey[itemKey]
		}
		else
		{
			
			fileLevel.deathBoxEntryDataByKey[itemKey] <- entryData
			entryData.key = itemKey
			entryData.lootFlav = lootFlavor
		}

		
		
		int otherLootEntIdx = 0
		bool closerIsBetter = GetCurrentPlaylistVarBool( "loba_ult_prefer_closer_loot", false ) 
		for ( ; otherLootEntIdx < entryData.lootEnts.len(); otherLootEntIdx++ )
		{
			entity otherLootEnt = entryData.lootEnts[otherLootEntIdx]

			if ( !IsValid( otherLootEnt ) )
				continue 

			bool continueForRestricted = false
			foreach ( int restrictedLootType in eRestrictedLootTypes )
			{
				entity restrictedPanel             = SURVIVAL_Loot_GetRestrictedPanel( restrictedLootType, lootEnt )
				bool lootEntIsRestrictedItem       = ( IsValid( restrictedPanel ) && SURVIVAL_Loot_IsRestrictedPanelLocked( restrictedLootType, restrictedPanel ) )
				entity otherLootEntRestrictedPanel = SURVIVAL_Loot_GetRestrictedPanel( restrictedLootType, otherLootEnt )
				bool otherLootEntIsRestrictedItem  = ( IsValid( otherLootEntRestrictedPanel ) && SURVIVAL_Loot_IsRestrictedPanelLocked( restrictedLootType, otherLootEntRestrictedPanel ) )
				if ( lootEntIsRestrictedItem && !otherLootEntIsRestrictedItem )
				{
					continueForRestricted = true 
					break
				}
				else if ( !lootEntIsRestrictedItem && otherLootEntIsRestrictedItem )
				{
					break 
				}
			}
			if ( continueForRestricted )
				continue
			else
			{
				if(isArmor)
				{
					
					if ( GetPropSurvivalMainPropertyFromEnt( lootEnt ) < GetPropSurvivalMainPropertyFromEnt( otherLootEnt ) )
						continue 

					else if ( GetPropSurvivalMainPropertyFromEnt( lootEnt ) <= GetPropSurvivalMainPropertyFromEnt( otherLootEnt ) && SURVIVAL_CreateLootRef(lootFlavor, lootEnt).lootExtraProperty > SURVIVAL_CreateLootRef(lootFlavor, otherLootEnt).lootExtraProperty )
						continue 
				}
				break
			}

			if ( hasSpecialAmmo && lootEnt.GetClipCount() < otherLootEnt.GetClipCount() )
				break 

			if ( isArmor && GetPropSurvivalMainPropertyFromEnt( lootEnt ) > GetPropSurvivalMainPropertyFromEnt( otherLootEnt ) )
				break 

			if ( closerIsBetter == (DistanceSqr( lootEnt.GetOrigin(), params.player.EyePosition() ) < DistanceSqr( otherLootEnt.GetOrigin(), params.player.EyePosition() )) )
				break
		}
		int lootEntPlacementIdx = otherLootEntIdx
		entryData.lootEnts.insert( lootEntPlacementIdx, lootEnt )

		lootEnt.e.deathBoxMenu_lastSeenClipCount = lootEnt.GetClipCount()

		fileLevel.deathBoxEntryDataByLootEnt[lootEnt] <- entryData

		entriesToUpdateSet[entryData] <- IN_SET
	}

	
	foreach ( entity deletedLootEnt, DeathBoxEntryData entryData in previousDeathBoxEntryDataByLootEnt )
	{
		entryData.lootEnts.removebyvalue( deletedLootEnt ) 
		entriesToUpdateSet[entryData] <- IN_SET

		delete fileLevel.deathBoxEntryDataByLootEnt[deletedLootEnt]
	}

	bool isBlackMarket = (deathBox.GetNetworkedClassName() == "prop_loot_grabber")

		bool isVendingMachine = false



		if ( isBlackMarket )
		{
			if ( IsVendingMachineUnsafe( deathBox ) )
			{
				isVendingMachine = true
				isBlackMarket = false
			}







		}

	
	bool deathBoxBlocked = false
	{
		string specialStateSamenessKey = ""

		array<entity> mainWeapons = SURVIVAL_GetPrimaryWeaponsIncludingSling( params.player, true )
		specialStateSamenessKey += mainWeapons.join( "|" )

		if ( isBlackMarket && GetBlackMarketUseCount( deathBox, GetLocalClientPlayer() ) >= GetBlackMarketUseLimit( deathBox, GetLocalClientPlayer() ) )
			deathBoxBlocked = true


		if ( !isBlackMarket && !fileLevel.insideNormalRange && !CanPlayerRemoteInteractWithDeathbox( GetLocalClientPlayer(), deathBox ) )
		{
			deathBoxBlocked = true
			RunUIScript( "CloseAllMenus" )
			return
		}


		specialStateSamenessKey += "|" + (deathBoxBlocked ? "blocked" : "usable")

		if ( specialStateSamenessKey != fileLevel.specialStateSamenessKey || fileLevel.predictedActionsDirty )
		{
			fileLevel.specialStateSamenessKey = specialStateSamenessKey
			fileLevel.predictedActionsDirty = false

			
			foreach ( string entryDataKey, DeathBoxEntryData entryData in fileLevel.deathBoxEntryDataByKey )
				entriesToUpdateSet[entryData] <- IN_SET
		}
	}

	
	string removeMode = GetCurrentPlaylistVarString( "death_box_menu_remove_on_taken", "mine" )
	foreach ( DeathBoxEntryData entryData, void _ in entriesToUpdateSet )
	{
		entity bestLootEnt = (entryData.lootEnts.len() > 0 && IsValid( entryData.lootEnts[0] ) ? entryData.lootEnts[0] : null)

		entryData.totalCount = 0
		bool isMainWeapon   = (entryData.lootFlav.lootType == eLootType.MAINWEAPON)
		bool hasSpecialAmmo = (isMainWeapon && !GetWeaponInfoFileKeyField_GlobalBool( entryData.lootFlav.baseWeapon, "uses_ammo_pool" ))
		bool isAmmo = (entryData.lootFlav.lootType == eLootType.AMMO)

		foreach ( entity lootEnt in entryData.lootEnts )
		{
			if ( !IsValid( lootEnt ) )
				continue

			if ( isMainWeapon )
				entryData.totalCount += 1
			else
				entryData.totalCount += lootEnt.GetClipCount()
		}

		int amountDelta             = entryData.lastSeenTotalCount - entryData.totalCount
		int amountPredictedPickedUp = 0
		for ( int predictedPickupIdx = 0; predictedPickupIdx < fileLevel.predictedActions.len(); predictedPickupIdx++ )
		{
			PredictedLootActionData plad = fileLevel.predictedActions[predictedPickupIdx]
			if ( plad.lootFlavRef != entryData.lootFlav.ref )
				continue

			if ( hasSpecialAmmo && plad.bestLootEntEEH != entryData.lastSeenBestLootEntEEH )
				continue


				
				if ( plad.actionIsFromVendingMachine )
				{
					fileLevel.predictedActions.remove( predictedPickupIdx )
					predictedPickupIdx-- 
					continue
				}


			if ( signum( amountDelta ) == signum( plad.count ) && abs( amountDelta ) >= abs( plad.count ) )
			{
				amountDelta -= plad.count
				fileLevel.predictedActions.remove( predictedPickupIdx )
				predictedPickupIdx-- 
			}
			else
			{
				amountPredictedPickedUp += plad.count
			}
		}
		entryData.lastSeenTotalCount = entryData.totalCount
		entryData.lastSeenBestLootEntEEH = (IsValid( bestLootEnt ) ? bestLootEnt.GetEncodedEHandle() : EncodedEHandle_null)

		entryData.totalCount = maxint( 0, entryData.totalCount - amountPredictedPickedUp )
		entryData.predictingNewEntToBeCreated = (amountPredictedPickedUp < 0)

		if ( entryData.totalCount > 0 )
			entryData.wasLastLootPickedUpByLocalPlayer = false

		if ( SURVIVAL_IsLootAnUpgrade( params.player, bestLootEnt, entryData.lootFlav, eLootContext.GROUND ) &&
				!LastItemWasBetterUpgrade( entryData.lootFlav , params.player, bestLootEnt ) )
		{
			entryData.isRelevant = true
			entryData.isUpgrade = (entryData.lootFlav.lootType != eLootType.MAINWEAPON && entryData.lootFlav.lootType != eLootType.AMMO)
		}
		else
		{
			entryData.isRelevant = !SURVIVAL_IsLootIrrelevant( params.player, bestLootEnt, entryData.lootFlav, eLootContext.GROUND ) &&
									!LastItemWasBetterUpgrade( entryData.lootFlav , params.player, bestLootEnt )
			entryData.isUpgrade = false
		}
		if (!(isAmmo && isBlackMarket))
			entryData.isBlocked = deathBoxBlocked

		bool shouldBeVisible = true
		if ( entryData.lootFlav.lootType == eLootType.AMMO )
		{
			
		}
		if ( entryData.lootFlav.lootType == eLootType.GADGET )
		{
			
		}
		else if ( removeMode == "all" )
		{
			shouldBeVisible = (entryData.totalCount > 0)
		}
		else if ( removeMode == "mine" )
		{
			shouldBeVisible = (entryData.totalCount > 0 || !entryData.wasLastLootPickedUpByLocalPlayer)
		}

		if ( entryData.lootFlav.lootType == eLootType.MAINWEAPON )
		{
			shouldBeVisible = entryData.lootEnts.len() > 0
		}


		if ( isVendingMachine && shouldBeVisible )
		{
			if ( !entryData.isRelevant )
			{
				shouldBeVisible = false
			}
			else if ( entryData.totalCount == 0 )
			{
				shouldBeVisible = false
			}
		}















		if ( entryData.lootFlav.lootType == eLootType.COLLECTABLE_NESSIE )
		{
			shouldBeVisible = false
		}



		if ( entryData.lootFlav.lootType == eLootType.CANDY_PICKUP )
		{
			shouldBeVisible = false
		}



		if ( entryData.lootFlav.lootType == eLootType.EVO_CACHE )
		{
			shouldBeVisible = false
		}


		bool shouldSortTop = false

		if ( entryData.lootFlav.lootType == eLootType.ARMOR )
		{
			shouldSortTop = true
		}


		DeathBoxListPanelItem ornull item = DeathBoxListPanel_GetItemByKey( fileLevel.listPanel, entryData.key )
		if ( shouldBeVisible )
		{
			if ( item == null )
			{
				int maxCategories = 99
				DeathBoxListPanelItem newItem
				int enumVal = GetPriorityForLootType( entryData.lootFlav )
				newItem.categoryKey = format( "%02d", maxCategories - enumVal )
				newItem.key = entryData.key
				newItem.sortOrdinal = format( "%02d,%02d,%02d,%d",
					shouldSortTop ? 99 : entryData.lootFlav.lootType,
					entryData.lootFlav.tier,
					int(newItem.categoryKey) == maxCategories ? GetWeaponCategorySortNumber( entryData ) : entryData.lootFlav.index,
					hasSpecialAmmo ? entryData.lastSeenBestLootEntEEH : 0
				)
				DeathBoxListPanel_AddItem( fileLevel.listPanel, newItem )
				item = newItem

				
				
				
				
				
				
				
				
				
				
			}
			UpdateItem( expect DeathBoxListPanelItem(item) )
		}
		else if ( item != null )
		{
			DeathBoxListPanel_RemoveItem( fileLevel.listPanel, expect DeathBoxListPanelItem(item) )
		}
	}

	array<entity> teammates   = []
	bool isMyTeamsBlackMarket = true
	var widgetRui             = Hud_GetRui( fileLevel.blackMarketWidget )
	if ( params.isBlackMarket )
	{

			Assert( !IsVendingMachine( deathBox ) )





		Hud_Show( fileLevel.blackMarketWidget )

		int localPlayerUseCount = GetBlackMarketUseCount( deathBox, GetLocalViewPlayer() )
		int useCountMax         = GetBlackMarketUseLimit( deathBox, GetLocalViewPlayer() )
		RuiSetInt( widgetRui, "useCountMax", useCountMax )
		isMyTeamsBlackMarket = (deathBox.GetTeam() == params.player.GetTeam()) 
		RuiSetBool( widgetRui, "isMyTeamsBlackMarket", isMyTeamsBlackMarket )

		HudElem_SetRuiArg( fileLevel.backer, "blackMarketStage", localPlayerUseCount == useCountMax ? 2 : localPlayerUseCount == useCountMax - 1 ? 1 : 0 )

		teammates = GetPlayerArrayOfTeam_Connected( params.player.GetTeam() )
		teammates.removebyvalue( params.player )
		teammates.sort( int function( entity a, entity b ) {
			return a.GetTeamMemberIndex() - b.GetTeamMemberIndex()
		} )
		teammates.insert( 0, params.player )
	}
	else
	{
		Hud_Hide( fileLevel.blackMarketWidget )
	}

	const int BLACK_MARKET_UI_TEAMMATES_SUPPORTED = 4
	float scaleFrac = GetScreenScaleFrac()

	for ( int playerIdx = 0; playerIdx < BLACK_MARKET_UI_TEAMMATES_SUPPORTED; playerIdx++ )
	{

		entity player = (playerIdx < teammates.len() && IsValid( teammates[playerIdx] ) ? teammates[playerIdx] : null)
		if ( !isMyTeamsBlackMarket && playerIdx > 0 )
			player = null 

		asset charImage           = $""
		asset charBackgroundImage = $""
		if ( player != null && LoadoutSlot_IsReady( ToEHI( player ), Loadout_Character() ) )
		{
			ItemFlavor char = LoadoutSlot_GetItemFlavor( ToEHI( player ), Loadout_Character() )
			charImage = CharacterClass_GetGalleryPortrait( char )
			charBackgroundImage = CharacterClass_GetGalleryPortraitBackground( char )

		}

		array<LootRef> useItemRefs = []
		if ( player != null )
			useItemRefs = GetBlackMarketUseItemRefs( deathBox, player )
		int BLACK_MARKET_UI_ITEMS_SUPPORTED = 3


			bool isPlayerLoba          = false
			bool hasBlackMarketUpgrade = false

			if ( player != null && IsValid( player ) )
			{
				hasBlackMarketUpgrade = player.HasPassive( ePassives.PAS_ULT_UPGRADE_ONE )
				isPlayerLoba = IsPlayerLoba( player )
			}


		if( playerIdx == 0  )
		{
			RuiSetString( widgetRui, format( "player%dName", playerIdx ), player != null ? GetDisplayablePlayerNameFromEHI( ToEHI( player ) ) : "" )
			RuiSetInt( widgetRui, format( "player%dUseCount", playerIdx ), player != null ? GetBlackMarketUseCount( deathBox, player ) : -1 )
			RuiSetFloat( widgetRui, format( "player%dLastPickupTime", playerIdx ), player != null ? GetBlackMarketLastUseTime( deathBox, player ) : -9999.0 )
			RuiSetAsset( widgetRui, format( "player%dCharImage", playerIdx ), charImage )
			RuiSetAsset( widgetRui, format( "player%dCharBGImage", playerIdx ), charBackgroundImage )
		}
		else
		{
			var unitFrameAttach    = Hud_GetChild( fileLevel.menu, format( "BlackMarketWidget_Player%d_UnitFrameAttach", playerIdx ) )
			Hud_SetVisible( unitFrameAttach, params.isBlackMarket && player != null)

			float unitFrameAttach_LeftPadding = 30
			float unitFrameAttach_RightPadding = 30
			float unitFrameAttach_ItemSize =  40
			float unitFrameAttach_ItemPadding = 7

			int itemsCount = 2

				itemsCount = ( ( isPlayerLoba && hasBlackMarketUpgrade )? 3: 2 )


			float unitFrameAttachSize = unitFrameAttach_LeftPadding
			unitFrameAttachSize += unitFrameAttach_RightPadding
			unitFrameAttachSize += unitFrameAttach_ItemSize * itemsCount
			unitFrameAttachSize += unitFrameAttach_ItemPadding * ( itemsCount - 1 )

			Hud_SetWidth( unitFrameAttach,  unitFrameAttachSize * scaleFrac  )
		}

		for ( int itemIdx = 0; itemIdx < BLACK_MARKET_UI_ITEMS_SUPPORTED; itemIdx++ )
		{
			var itemButton    = Hud_GetChild( fileLevel.menu, format( "BlackMarketWidget_Player%d_ItemButton%d", playerIdx, itemIdx ) )
			var itemButtonRui = Hud_GetRui( itemButton )

			Hud_ClearToolTipData( itemButton )
			Hud_SetVisible( itemButton, player != null )

			int itemButtonOffset          = Hud_GetBaseX( itemButton )

			if ( playerIdx == 1 && itemIdx == 0 )
				itemButtonOffset = teammates.len() > 2 ? itemButtonOffset : 9

			float itemButtonSizeRatio     = 1.0
			bool isItemButtonBonusVisible = false
			bool isItemButtonBonus        = itemIdx == 2

			if( isPlayerLoba )
			{
				if( playerIdx == 0 ) 
				{
					isItemButtonBonusVisible = isPlayerLoba
					itemButtonSizeRatio      = 0.85

					if( itemIdx == 0 )
						itemButtonOffset += int( -46 * scaleFrac )
				}
				else
				{
					isItemButtonBonusVisible = isPlayerLoba && hasBlackMarketUpgrade
				}
			}

			if ( isItemButtonBonus )
				Hud_SetVisible( itemButton, isItemButtonBonusVisible )



			Hud_SetX( itemButton, itemButtonOffset )
			Hud_SetWidth( itemButton, Hud_GetBaseWidth( itemButton ) * itemButtonSizeRatio  )
			Hud_SetHeight( itemButton, Hud_GetBaseHeight( itemButton ) * itemButtonSizeRatio  )

			if ( itemIdx >= useItemRefs.len() || useItemRefs[itemIdx].lootData.index < 0 )
			{
				RunUIScript( "ClientToUI_Tooltip_Clear", itemButton )

					if( isItemButtonBonus && !hasBlackMarketUpgrade )
					{
						ToolTipData toolTipData
						toolTipData.titleText = "#UPGRADE_LOBA_ULT_GROUND_LIST_TOOLTIP_TITLE"
						toolTipData.descText = "#UPGRADE_LOBA_ULT_GROUND_LIST_TOOLTIP_DESC"
						toolTipData.tooltipFlags = toolTipData.tooltipFlags | eToolTipFlag.PING_DISSABLED
						Hud_SetToolTipData( itemButton, toolTipData )
						RunUIScript( "ClientToUI_Tooltip_MarkForClientUpdate", itemButton, eTooltipStyle.DEFAULT )
						Hud_SetLocked( itemButton, true )
					}
					else

					Hud_SetLocked( itemButton, false )

				RuiSetImage( itemButtonRui, "iconImage", $"" )
				RuiSetInt( itemButtonRui, "lootType", eLootType.INVALID )
				RuiSetInt( itemButtonRui, "lootTier", 0 )
				RuiSetInt( itemButtonRui, "count", 0 )
				RuiSetImage( itemButtonRui, "ammoTypeImage", $"" )

				continue
			}

			LootData lootFlavor = useItemRefs[itemIdx].lootData

			Hud_SetEnabled( itemButton, true )
			Hud_SetLocked( itemButton, false )
			Hud_SetSelected( itemButton, false )

			asset icon     = lootFlavor.hudIcon
			asset ammoIcon = $""
			if ( lootFlavor.lootType == eLootType.MAINWEAPON )
			{
				ItemFlavor ornull weaponFlav = GetWeaponItemFlavorByClass( lootFlavor.baseWeapon )


					icon = GetWeaponLootIcon_WeaponLootHint( player, lootFlavor )





				ammoIcon = lootFlavor.fakeAmmoIcon == $"" ? $"rui/hud/gametype_icons/survival/sur_ammo_unique" : lootFlavor.fakeAmmoIcon
				string ammoType = GetWeaponAmmoType( lootFlavor.ref )
				if ( GetWeaponInfoFileKeyField_GlobalBool( lootFlavor.baseWeapon, "uses_ammo_pool" ) )
				{
					LootData ammoData = SURVIVAL_Loot_GetLootDataByRef( ammoType )
					ammoIcon = ammoData.hudIcon
				}
			}


			RuiSetImage( itemButtonRui, "iconImage", icon )
			RuiSetImage( itemButtonRui, "ammoTypeImage", ammoIcon )
			RuiSetInt( itemButtonRui, "lootType", lootFlavor.lootType )
			RuiSetInt( itemButtonRui, "lootTier", lootFlavor.tier )
			RuiSetInt( itemButtonRui, "count", useItemRefs[itemIdx].count )
			RuiSetBool( itemButtonRui, "isFullyKitted", lootFlavor.lootType == eLootType.MAINWEAPON && lootFlavor.tier == 4 )

			ToolTipData toolTipData
			toolTipData.titleText = lootFlavor.pickupString
			toolTipData.descText = SURVIVAL_Loot_GetPickupString( lootFlavor, params.player )
			
			toolTipData.tooltipFlags = toolTipData.tooltipFlags | eToolTipFlag.PING_DISSABLED
			Hud_SetToolTipData( itemButton, toolTipData )
			RunUIScript( "ClientToUI_Tooltip_MarkForClientUpdate", itemButton, eTooltipStyle.DEFAULT )
		}
	}
}


bool function IsPlayerLoba( entity player )
{
	if ( !IsValid( player ) )
		return false

	ItemFlavor character = LoadoutSlot_GetItemFlavor( ToEHI( player ), Loadout_Character() )
	string characterRef  = ItemFlavor_GetCharacterRef( character ).tolower()

	if ( characterRef == "character_loba" )
		return true

	return false
}


int function GetWeaponCategorySortNumber( DeathBoxEntryData entryData )
{
#if DEV
		printt( format( "%s(): Getting SortOrdinal Substring for: %s", FUNC_NAME(), entryData.key ) )
#endif

	
	int specialAmmoNDX = entryData.key.find( "specialammo" )
	if( specialAmmoNDX >= 0 )
	{
		string ammoType = entryData.lootFlav.ammoType
#if DEV
			printt( format( "%s(): Crate Weapon! Ammo Type == %s", FUNC_NAME(), ammoType ) )
#endif
		return( StringHash( entryData.lootFlav.ammoType ) )
	}

	table< string, int > ammoRanking
	ammoRanking[ SNIPER_AMMO ] 	<- 10000 - 1000
	ammoRanking[ ARROWS_AMMO ] 	<- 10000 - 2000
	ammoRanking[ HIGHCAL_AMMO ] <- 10000 - 3000
	ammoRanking[ SPECIAL_AMMO ] <- 10000 - 4000
	ammoRanking[ BULLET_AMMO ] 	<- 10000 - 5000
	ammoRanking[ SHOTGUN_AMMO ] <- 10000 - 6000




	return( ammoRanking[ entryData.lootFlav.ammoType ] )
}




bool function LastItemWasBetterUpgrade( LootData data, entity player, entity bestLootEnt )
{
	if ( fileLevel.predictedActions.len() == 0 || !IsValid( player ) || !IsValid( bestLootEnt ) )
		return false

	if ( data.lootType == eLootType.MAINWEAPON )
		return false

	LootData lastLootItem = SURVIVAL_Loot_GetLootDataByRef( fileLevel.predictedActions.top().lootFlavRef )

	return IsValid( lastLootItem ) && lastLootItem.attachmentStyle == data.attachmentStyle &&
			SURVIVAL_IsLootAnUpgrade( player, bestLootEnt, lastLootItem, eLootContext.GROUND ) &&
			lastLootItem.tier > data.tier
}




void function UpdateItem( DeathBoxListPanelItem item )
{
	var button        = item.allocatedButton
	entity viewPlayer = GetLocalViewPlayer()

	if ( !(item.key in fileLevel.deathBoxEntryDataByKey) )
	{
		foreach ( string entryDataKey, DeathBoxEntryData entryData in fileLevel.deathBoxEntryDataByKey )
			printf( "Debug death box item: " + entryDataKey )

		Assert( item.key in fileLevel.deathBoxEntryDataByKey, "Debug death box item: " + item.key + "does not exist" )
		return
	}

	DeathBoxEntryData entryData = fileLevel.deathBoxEntryDataByKey[item.key]

	LootData lootFlavor   = entryData.lootFlav
	LootTypeData lootType = GetLootTypeData( lootFlavor.lootType )

	Signal( entryData, "DeathBoxEntryData_Update" )

	if ( button == null )
		return

	var rui = Hud_GetRui( button )

	bool isAmmo         = (lootFlavor.lootType == eLootType.AMMO)
	bool isMainWeapon   = (lootFlavor.lootType == eLootType.MAINWEAPON)
	bool isGadget		= (lootFlavor.lootType == eLootType.GADGET)
	bool isArmor   		= (lootFlavor.lootType == eLootType.ARMOR)

	bool isHealth  		= ( (lootFlavor.lootType == eLootType.HEALTH) && lootFlavor.ref != "health_pickup_ultimate" )




	bool hasSpecialAmmo = (isMainWeapon && !GetWeaponInfoFileKeyField_GlobalBool( lootFlavor.baseWeapon, "uses_ammo_pool" ))

	bool pingsAreIndirect = false
	bool isBlackMarket    = false
	entity deathBox       = GetEntityFromEncodedEHandle( fileLevel.currentDeathBoxEEH )
	if ( IsValid( deathBox ) && deathBox.GetNetworkedClassName() == "prop_loot_grabber" )
	{
		isBlackMarket = true
		pingsAreIndirect = true
	}

		bool isVendingMachine = false



		if ( isBlackMarket )
		{
			if ( IsVendingMachineUnsafe( deathBox ) )
			{
				isVendingMachine = true
				isBlackMarket = false
			}







		}


	array<entity> pingWaypoints = []
	int team                    = viewPlayer.GetTeam()
	foreach ( entity lootEnt in entryData.lootEnts )
	{
		if ( !IsValid( lootEnt ) )
			continue

		array<entity> lootEntPingWaypoints = Waypoint_GetWaypointsForLootItemPingedByTeam( lootEnt, team, pingsAreIndirect )
		pingWaypoints.extend( lootEntPingWaypoints )
	}


	if ( isVendingMachine )
	{
		RuiSetInt( rui, "count", 1 )
	}
	else

	{
		RuiSetInt( rui, "count", hasSpecialAmmo ? 1 : entryData.totalCount )
	}

	bool isPinged     = false
	bool isPingedByUs = false
	foreach ( entity pingWaypoint in pingWaypoints )
	{
		if ( Waypoint_IsHiddenFromLocalHud( pingWaypoint ) )
			continue

		isPinged = true
		if ( pingWaypoint.GetOwner() == viewPlayer )
			isPingedByUs = true
	}

	RuiSetBool( rui, "isPinged", isPinged )
	if ( isPinged )
		thread ItemPingUpdateThread( item, entryData, pingWaypoints )
	else
		RuiSetAsset( rui, "dibsAvatar", $"" )

	string title = lootFlavor.pickupString
	if ( isAmmo )
	{

		 if ( isVendingMachine )
		 {
			 title = ""
		 }
		else

		{
			title = (entryData.totalCount == 0 ? "--" : format( "%d", entryData.totalCount ))
		}
	}

	if ( isHealth )
	{

		 if ( isVendingMachine )
		 {
			 title = ""
		 }
		else

		{
			title = (entryData.totalCount == 0 ? "--" : format( "%d", entryData.totalCount ))
		}
	}


	else if ( isVendingMachine && isMainWeapon )
	{

			lootFlavor.hudIcon = GetWeaponLootIcon_WeaponLootHint( GetLocalClientPlayer(), lootFlavor )

	}

	else if ( hasSpecialAmmo )
	{
		Assert( entryData.lootEnts.len() <= 1 )
		if ( entryData.lootEnts.len() > 0 )
		{
			entity lootEnt = entryData.lootEnts[0]
			if ( IsValid( lootEnt ) )
			{
				int maxAmmo  = GetWeaponInfoFileKeyField_GlobalInt( lootFlavor.baseWeapon, "ammo_default_total" )
				int currAmmo = lootEnt.GetClipCount()
				if ( currAmmo < 0 )
					currAmmo = maxAmmo

				title = Localize( "#HUD_LOOT_WITH_CLIP", Localize( lootFlavor.pickupString ), format( "%d/%d", currAmmo, maxAmmo ) )
			}
		}
	}
	else if ( isMainWeapon )
	{
		title = (entryData.totalCount > 1 ? format( "%s   x%d", title, entryData.totalCount ) : title)


			lootFlavor.hudIcon = GetWeaponLootIcon_WeaponLootHint( GetLocalClientPlayer(), lootFlavor )

	}
	else if ( lootFlavor.passive != ePassives.INVALID )
	{
		string passiveName = PASSIVE_NAME_MAP[lootFlavor.passive]
		title = Localize( "#HUD_LOOT_WITH_PASSIVE", Localize( lootFlavor.pickupString ), Localize( passiveName ) )
	}
	RuiSetString( rui, "buttonText", title )

	RuiSetImage( rui, "iconImage", lootFlavor.hudIcon )
	RuiSetInt( rui, "lootTier", lootFlavor.tier )
	RuiSetInt( rui, "lootType", lootFlavor.lootType )

	asset ammoTypeIcon = $""
	if ( hasSpecialAmmo )
	{
		ammoTypeIcon = lootFlavor.fakeAmmoIcon
	}
	else if ( isMainWeapon && SURVIVAL_Loot_IsRefValid( lootFlavor.ammoType ) )
	{
		LootData ammoData = SURVIVAL_Loot_GetLootDataByRef( lootFlavor.ammoType )
		ammoTypeIcon = ammoData.hudIcon
	}
	RuiSetAsset( rui, "ammoTypeImage", ammoTypeIcon )

	entity bestLootEnt = (entryData.lootEnts.len() > 0 && IsValid( entryData.lootEnts[0] ) ? entryData.lootEnts[0] : null)
	entryData.isUsable = (entryData.totalCount > 0 || isGadget ) && (IsValid( bestLootEnt ) || entryData.predictingNewEntToBeCreated)

	bool isBackpackItem = ( lootType.index != eLootType.MAINWEAPON && lootType.equipmentSlot == "" && lootFlavor.inventorySlotCount > 0 )
	entryData.isClickable = entryData.isUsable && !entryData.isBlocked && ( entryData.isRelevant || isBackpackItem )


	if ( !fileLevel.insideNormalRange )
	{
		if ( isArmor && !RemoteDeathboxInteractAllowTakingArmor() )
		{
			entryData.isBlocked = true
		}

		if ( isMainWeapon && !RemoteDeathboxInteractAllowTakingGuns() )
		{
			entryData.isBlocked = true
		}
	}


	Hud_SetEnabled( button, true )
	RuiSetBool( rui, "isUsable", entryData.isUsable )
	Hud_SetLocked( button, !entryData.isUsable )
	RuiSetBool( rui, "isRelevant", entryData.isRelevant )
	RuiSetBool( rui, "isUpgrade", entryData.isUpgrade )
	RuiSetBool( rui, "isDimmed", (fileLevel.currentQuickSwapEntry != null && fileLevel.currentQuickSwapEntry != entryData) )
	RuiSetBool( rui, "isBlocked", entryData.isBlocked )
	RuiSetBool( rui, "isClickable", entryData.isClickable )

	RuiSetBool( rui, "isHealthItem", isHealth )







	if(bestLootEnt != null && isArmor)
	{
		
		bool isEvolving = EvolvingArmor_IsEquipmentEvolvingArmor( lootFlavor.ref )

		isEvolving = isEvolving || UpgradeCore_IsEquipmentArmorCore( lootFlavor.ref )


		entity localPlayer = GetLocalClientPlayer()
		int armorTier = EquipmentSlot_GetEquipmentTier( localPlayer, "armor" )

		RuiSetFloat( rui, "shieldFrac", float(GetPropSurvivalMainPropertyFromEnt( bestLootEnt )) )
		RuiSetBool( rui, "isEvolvingShield", isEvolving )
		RuiSetInt( rui, "shieldEvolutionProgress", SURVIVAL_CreateLootRef(lootFlavor, bestLootEnt).lootExtraProperty)
		RuiSetInt( rui, "replaceShieldEvolutionProgress", EvolvingArmor_GetEvolutionProgress(localPlayer))
		RuiSetInt( rui, "lootTierReplace", armorTier )
	}















	entryData.isRestrictedItem = false
	entryData.restrictedType = NORMAL_UNRESTRICTED_LOOT

	bool hasTooltip = false
	if ( IsValid( bestLootEnt ) && entryData.isUsable )
	{
		if ( isBlackMarket )
		{
			foreach ( int restrictedLootType in eRestrictedLootTypes )
			{
				entity restrictedPanel = SURVIVAL_Loot_GetRestrictedPanel( restrictedLootType, bestLootEnt )
				if ( IsValid( restrictedPanel ) && SURVIVAL_Loot_IsRestrictedPanelLocked( restrictedLootType, restrictedPanel ) && lootType.index != eLootType.AMMO )
				{
					entryData.isRestrictedItem = true
					entryData.restrictedType = restrictedLootType
					break
				}
			}
		}

		if ( !entryData.isBlocked )
		{
			hasTooltip = true
			ToolTipData dt
			dt.tooltipStyle = isMainWeapon ? eTooltipStyle.WEAPON_LOOT_PROMPT : eTooltipStyle.LOOT_PROMPT
			dt.lootPromptData.lootContext = eLootContext.GROUND
			dt.lootPromptData.index = lootFlavor.index

				dt.lootPromptData.count = isVendingMachine ? 1 : entryData.totalCount



			dt.lootPromptData.isPinged = isPinged
			dt.lootPromptData.isPingedByUs = isPingedByUs
			dt.lootPromptData.isInDeathBox = true
			dt.tooltipFlags = dt.tooltipFlags | (IsPingEnabledForPlayer( viewPlayer ) ? eToolTipFlag.PING_DISSABLED : 0)
			
			{
				dt.lootPromptData.guid = bestLootEnt.GetEncodedEHandle()
				dt.lootPromptData.property = GetPropSurvivalMainPropertyFromEnt( bestLootEnt )
				if ( isMainWeapon )
					dt.lootPromptData.mods = bestLootEnt.GetWeaponMods()
			}
			Hud_SetToolTipData( button, dt )
			RunUIScript( "ClientToUI_Tooltip_MarkForClientUpdate", button, isMainWeapon ? eTooltipStyle.WEAPON_LOOT_PROMPT : eTooltipStyle.LOOT_PROMPT )
		}
	}

	if ( !hasTooltip )
	{
		Hud_ClearToolTipData( button )
		RunUIScript( "ClientToUI_Tooltip_Clear", button )
	}

	RuiSetBool( rui, "isSpecialItem", entryData.isRestrictedItem )

	RuiSetInt( rui, "useCounter", entryData.useCounter )
	RuiSetInt( rui, "pingCounter", entryData.pingCounter )
}




void function ServerToClient_UpdateItem( entity lootEnt )
{
	if ( IsValid( lootEnt ) )
	{
		LootData lootFlavor = SURVIVAL_Loot_GetLootDataByIndex( lootEnt.GetSurvivalInt() )

		bool hasSpecialAmmo = (lootFlavor.lootType == eLootType.MAINWEAPON && !GetWeaponInfoFileKeyField_GlobalBool( lootFlavor.baseWeapon, "uses_ammo_pool" ))
		string itemKey = (hasSpecialAmmo ? format( "specialammo%d", lootEnt.GetEncodedEHandle() ) : lootFlavor.ref)

		if ( fileLevel.listPanel != null )
		{
			DeathBoxListPanelItem ornull item = DeathBoxListPanel_GetItemByKey( fileLevel.listPanel, itemKey )
			if ( item != null )
			{
				UpdateItem( expect DeathBoxListPanelItem(item) )
			}
		}
	}
}




void function ItemPingUpdateThread( DeathBoxListPanelItem item, DeathBoxEntryData entryData, array<entity> pingWaypoints )
{
	EndSignal( fileLevel.signalDummy, "SurvivalGroundList_Closed" )
	EndSignal( entryData, "DeathBoxEntryData_Update" )
	EndSignal( entryData, "DeathBoxEntryData_Unbind" )

	var button = item.allocatedButton
	var rui    = Hud_GetRui( button )

	while ( true )
	{
		ArrayRemoveInvalid( pingWaypoints )
		if ( pingWaypoints.len() == 0 )
			break

		entity dibsPlayer = null
		foreach ( entity pingWaypoint in pingWaypoints )
		{
			entity pingWaypointDibsPlayer = Waypoint_GetLootPingDibsPlayer( pingWaypoint )
			if ( IsValid( pingWaypointDibsPlayer ) )
				dibsPlayer = pingWaypointDibsPlayer
		}

		asset dibsPlayerIcon = $""
		if ( IsValid( dibsPlayer ) && LoadoutSlot_IsReady( ToEHI( dibsPlayer ), Loadout_Character() ) )
			dibsPlayerIcon = CharacterClass_GetGalleryPortrait( LoadoutSlot_GetItemFlavor( ToEHI( dibsPlayer ), Loadout_Character() ) )
		RuiSetAsset( rui, "dibsAvatar", dibsPlayerIcon )

		WaitFrame()
	}

	entryData.pingCounter = 0
	if ( item.key in fileLevel.deathBoxEntryDataByKey )
	{
		thread UpdateItem( item ) 
	}
	
}




bool function IsGroundListMenuOpen()
{
	return IsValid( fileLevel.menu ) && Hud_IsVisible( fileLevel.menu )
}




void function ItemButtonSetup( var button )
{
	RunUIScript( "Survival_CommonButtonInit", button )
}




void function OnCategoryHeaderBind( DeathBoxListPanelData listPanelData, DeathBoxListPanelCategory category )
{
	RuiSetColorAlpha( Hud_GetRui( category.allocatedHeaderPanel ), "categoryHeaderTextCol", fileLevel.categoryHeaderTextCol, 1.0 )

	entity deathBox    = GetEntityFromEncodedEHandle( fileLevel.currentDeathBoxEEH )
	bool isBlackMarket = false
	if ( IsValid( deathBox ) && deathBox.GetNetworkedClassName() == "prop_loot_grabber")
	{

		if ( IsVendingMachineUnsafe( deathBox ) )
		{
			
		}






		else

		{
			isBlackMarket = true
		}
	}

	RuiSetBool( Hud_GetRui( category.allocatedHeaderPanel ), "isBlackMarket", isBlackMarket )
}




void function OnCategoryHeaderUnbind( DeathBoxListPanelData listPanelData, DeathBoxListPanelCategory category )
{
	
}




void function OnItemBind( DeathBoxListPanelData listPanelData, DeathBoxListPanelItem item )
{
	UpdateItem( item )
}




void function OnItemUnbind( DeathBoxListPanelData listPanelData, DeathBoxListPanelItem item )
{
	if ( !(item.key in fileLevel.deathBoxEntryDataByKey) )
		return

	DeathBoxEntryData entryData = fileLevel.deathBoxEntryDataByKey[item.key]
	Signal( entryData, "DeathBoxEntryData_Unbind" )
}




void function OnItemClick( DeathBoxListPanelData listPanelData, DeathBoxListPanelItem item )
{
	CloseQuickSwapIfOpen()

	bool isAltAction            = false
	bool fromExtendedUse        = false
	bool isFromQuickSwap        = false
	int ornull backpackSwapSlot = null
	PerformItemAction( item, isAltAction, fromExtendedUse, isFromQuickSwap, backpackSwapSlot )
}




void function OnItemClickRight( DeathBoxListPanelData listPanelData, DeathBoxListPanelItem item )
{
	CloseQuickSwapIfOpen()

	bool isAltAction            = true
	bool fromExtendedUse        = false
	bool isFromQuickSwap        = false
	int ornull backpackSwapSlot = null
	PerformItemAction( item, isAltAction, fromExtendedUse, isFromQuickSwap, backpackSwapSlot )
}




void functionref() wtfLootVaultConfirmDialogDoItFunc = null
void function PerformItemAction( DeathBoxListPanelItem item, bool isAltAction, bool fromExtendedUse, bool isFromQuickSwap, int ornull backpackSwapSlot )
{
	entity player               = GetLocalClientPlayer()
	DeathBoxEntryData entryData = fileLevel.deathBoxEntryDataByKey[item.key]
	if ( !entryData.isClickable )
		return

	LootData lootFlavor = entryData.lootFlav
	entity bestLootEnt  = ( entryData.lootEnts.len() > 0 && IsValid( entryData.lootEnts[0] ) ? entryData.lootEnts[0] : null )

	if( !IsValid( bestLootEnt ) )
		return

	entity deathBox    = GetEntityFromEncodedEHandle( fileLevel.currentDeathBoxEEH )
	bool isBlackMarket = false
	bool needExtendedUse = false

	bool remoteInteracting = false

	if ( IsValid( deathBox ) )
	{
		if ( deathBox.GetNetworkedClassName() == "prop_loot_grabber" )
		{
			isBlackMarket = true


			if ( IsVendingMachineUnsafe( deathBox ) )
			{
				needExtendedUse = false
			}







			else

			{
				needExtendedUse = true
			}
		}

		else if ( !IsPlayerWithinStandardDeathBoxUseDistance( player, deathBox ) )
		{
			remoteInteracting = true
		}

	}

	int count = SURVIVAL_GetInventorySlotCountForPlayer( player, entryData.lootFlav )

	if ( entryData.lootFlav.lootType == eLootType.AMMO && !isBlackMarket && !remoteInteracting )



	{
		int countToFillStack = SURVIVAL_GetCountToFillStack( player, entryData.lootFlav.ref )
		if ( countToFillStack < count )
			count += countToFillStack 
	}

	array<ConsumableInventoryItem> predictedInventory = GetPredictedInventory()
	int inventoryLimit                                = SURVIVAL_GetInventoryLimit( player )
	int amountThatWouldBePickedUp                     = SURVIVAL_AddToInventory( player, predictedInventory, inventoryLimit, lootFlavor, count, SURVIVAL_GetInventorySlotCountForPlayer( player, lootFlavor ), true )
	bool isInventoryFull                              = (amountThatWouldBePickedUp == 0)

	LootRef lootRef  = SURVIVAL_CreateLootRef( lootFlavor, bestLootEnt )
	int actionType = isAltAction ? eLootActionType.ALT_ACTION : eLootActionType.PRIMARY_ACTION
	int groundAction = SURVIVAL_GetActionForGroundItem( player, lootRef, actionType ).action

	bool showUseHighlight     = false
	bool shouldCloseQuickSwap = true

	if ( groundAction == eLootAction.NONE || groundAction == eLootAction.IGNORE )
	{
		
	}
	else if ( !fromExtendedUse && !isFromQuickSwap && ( ShouldStartExtendedUseForGroundPaneltemAction( groundAction ) || needExtendedUse) )
	{
		bool requiresButtonFocus = true
		float duration = 0.4
		StartMenuExtendedUse( item.allocatedButton, fileLevel.holdToUseElem, duration, requiresButtonFocus, isAltAction, void function( bool success ) : (item, isAltAction, backpackSwapSlot) {
			if ( success )
				PerformItemAction( item, isAltAction, true, false, backpackSwapSlot )
		}, "UI_Survival_PickupTicker", "" )
	}
	else if ( count > 0 && isInventoryFull && backpackSwapSlot == null && groundAction == eLootAction.PICKUP && lootRef.lootData.ref != "s4t_item0" && lootRef.lootData.lootType != eLootType.GADGET )
	{
		OpenQuickSwap( item.allocatedButton, entryData, isAltAction )
		showUseHighlight = true
		shouldCloseQuickSwap = false
	}
	else
	{
		void functionref() doIt = void function() : ( entryData, count, bestLootEnt, groundAction, isAltAction, backpackSwapSlot ) {
			SendItemActionCommand( entryData, count, bestLootEnt, groundAction, isAltAction, backpackSwapSlot )
		}

		if ( isBlackMarket && entryData.isRestrictedItem )
		{
			wtfLootVaultConfirmDialogDoItFunc = doIt
			RunUIScript( "ClientToUI_RestrictedLootConfirmDialog_Open", deathBox.GetOwner() == player, entryData.restrictedType )
		}
		else
		{
			doIt()
		}

		showUseHighlight = true
	}

	if ( showUseHighlight )
	{
		entryData.useCounter += 1
		UpdateItem( item )
		EmitUISound( "ui_menu_accept" )
	}

	if ( shouldCloseQuickSwap )
		CloseQuickSwapIfOpen()
}

bool function ShouldStartExtendedUseForGroundPaneltemAction( int groundAction )
{
	if( groundAction == eLootAction.SWAP )
		return true

	if( groundAction == eLootAction.SWAP_TO_SLING )
		return true

	return false
}
































void function UIToClient_RestrictedLootConfirmDialog_DoIt()
{
	wtfLootVaultConfirmDialogDoItFunc()
	wtfLootVaultConfirmDialogDoItFunc = null
}



array<ConsumableInventoryItem> function GetPredictedInventory()
{
	entity localPlayer                                = GetLocalClientPlayer()
	int inventoryLimit                                = SURVIVAL_GetInventoryLimit( localPlayer )
	array<ConsumableInventoryItem> predictedInventory = SURVIVAL_GetPlayerInventory( localPlayer )

	foreach ( PredictedLootActionData plad in fileLevel.predictedActions )
	{
		LootData pladLootFlav = SURVIVAL_Loot_GetLootDataByRef( plad.lootFlavRef )

		if ( plad.count > 0 )
			SURVIVAL_AddToInventory( localPlayer, predictedInventory, inventoryLimit, pladLootFlav, plad.count, SURVIVAL_GetInventorySlotCountForPlayer( localPlayer, pladLootFlav ), true )
		else
			SURVIVAL_RemoveFromInventory( localPlayer, predictedInventory, pladLootFlav, -plad.count )
	}

	return predictedInventory
}




void function SendItemActionCommand( DeathBoxEntryData entryData, int count, entity preferredLootEnt, int actionType, bool isAltAction, int ornull backpackSwapSlot )
{
	count = maxint( 1, count )

	entity localPlayer = GetLocalClientPlayer()
	int pickupFlags    = PICKUP_FLAG_FROM_MENU
	if ( isAltAction )
		pickupFlags = pickupFlags | PICKUP_FLAG_ALT
	if ( actionType == eLootAction.ATTACH_TO_ACTIVE )
		pickupFlags = pickupFlags | PICKUP_FLAG_ATTACH_ACTIVE_ONLY
	if ( actionType == eLootAction.ATTACH_TO_STOWED )
		pickupFlags = pickupFlags | PICKUP_FLAG_ATTACH_STOWED_ONLY

	entity boxEnt = GetEntityFromEncodedEHandle( fileLevel.currentDeathBoxEEH )
	if ( IsValid( boxEnt ) )
	{
		switch ( boxEnt.GetNetworkedClassName() )
		{
			case "prop_death_box":
			{
				Remote_ServerCallFunction( "ClientCallback_PickupSurvivalLootFromDeathbox",
					entryData.lootFlav.index,
					count,
					preferredLootEnt,
					boxEnt,
					pickupFlags,
							( backpackSwapSlot != null ? expect int( backpackSwapSlot ) : -1 )
				)
				break
			}
			case "prop_loot_grabber":
			{
				Remote_ServerCallFunction( "ClientCallback_PickupSurvivalLootFromLootGrabber",
					entryData.lootFlav.index,
					count,
					preferredLootEnt,
					boxEnt,
					pickupFlags,
							( backpackSwapSlot != null ? expect int( backpackSwapSlot ) : -1 )
				)
				break
			}
			default:
			{
				Assert( false, "Attempted to pick up from non known box entity of type: " + string( boxEnt.GetNetworkedClassName() ) )
				break
			}
		}
	}

	PredictedLootActionData plad
	plad.type = actionType
	plad.time = Time()
	plad.lootFlavRef = entryData.lootFlav.ref
	plad.bestLootEntEEH = IsValid( preferredLootEnt ) ? preferredLootEnt.GetEncodedEHandle() : EncodedEHandle_null
	plad.backpackSwapSlot = backpackSwapSlot


	if ( IsVendingMachine( boxEnt ) )
	{
		plad.actionIsFromVendingMachine = true
	}


	plad.count = minint( count, entryData.totalCount )
	if ( actionType == eLootAction.PICKUP || actionType == eLootAction.PICKUP_ALL || actionType == eLootAction.SWAP )
	{
		array<ConsumableInventoryItem> predictedInventory = GetPredictedInventory()
		int inventoryLimit = SURVIVAL_GetInventoryLimit( localPlayer )
		int numAdded       = SURVIVAL_AddToInventory( localPlayer, predictedInventory, inventoryLimit, entryData.lootFlav, plad.count, SURVIVAL_GetInventorySlotCountForPlayer( localPlayer, entryData.lootFlav ), true )
		plad.count = numAdded
	}

	fileLevel.predictedActions.append( plad )
	fileLevel.predictedActionsDirty = true

	if ( plad.count >= entryData.totalCount )
		entryData.wasLastLootPickedUpByLocalPlayer = true
}




void function OnItemFocusGet( DeathBoxListPanelData listPanelData, DeathBoxListPanelItem item )
{
	RunUIScript( "UpdateFooterOptions" )
}




void function OnItemFocusLose( DeathBoxListPanelData listPanelData, DeathBoxListPanelItem item )
{
	RunUIScript( "UpdateFooterOptions" )
}




bool function OnItemKeyEvent( DeathBoxListPanelData listPanelData, DeathBoxListPanelItem item, int keyId, bool isDown )
{
	if ( keyId == BUTTON_SHOULDER_RIGHT && isDown )
	{
		PingItem( item )
		return true
	}

	
	
	
	
	
	

	return false
}




void function OnItemCommandEvent( DeathBoxListPanelData listPanelData, DeathBoxListPanelItem item, string command )
{
	if ( command == "+ping" )
		PingItem( item )

}




void function PingItem( DeathBoxListPanelItem item )
{
	DeathBoxEntryData entryData = fileLevel.deathBoxEntryDataByKey[item.key]
	entity bestLootEnt          = (entryData.lootEnts.len() > 0 && IsValid( entryData.lootEnts[0] ) ? entryData.lootEnts[0] : null)
	if ( !IsValid( bestLootEnt ) )
		return

	entity deathBox = GetEntityFromEncodedEHandle( fileLevel.currentDeathBoxEEH )

	bool pingsAreIndirect = false
	if ( IsValid( deathBox ) && deathBox.GetNetworkedClassName() == "prop_loot_grabber" )
		pingsAreIndirect = true

	entity localPlayer       = GetLocalClientPlayer()
	entity lootEntPingedByUs = null
	foreach ( entity lootEnt in entryData.lootEnts )
	{
		if ( IsValid( lootEnt ) && IsValid( Waypoint_GetWaypointForLootItemPingedByPlayer( lootEnt, localPlayer, pingsAreIndirect ) ) )
		{
			lootEntPingedByUs = lootEnt
			break
		}
	}

	if ( IsValid( lootEntPingedByUs ) )
	{
		
		PingGroundLoot( lootEntPingedByUs, deathBox )
		entryData.pingCounter = 0
	}
	else
	{
		
		PingGroundLoot( bestLootEnt, deathBox )
		entryData.pingCounter += 1
	}
	UpdateItem( item )
}




void function OnPingCreatedByAnyPlayer( entity pingingPlayer, int pingType, entity focusEnt, vector pingLoc, entity wayPoint )
{
	if ( pingType != ePingType.LOOT )
		return

	entity lootEnt = Waypoint_GetItemEntForLootWaypoint( wayPoint )
	if ( !(lootEnt in fileLevel.deathBoxEntryDataByLootEnt) )
		return
	DeathBoxEntryData entryData       = fileLevel.deathBoxEntryDataByLootEnt[lootEnt]
	DeathBoxListPanelItem ornull item = DeathBoxListPanel_GetItemByKey( fileLevel.listPanel, entryData.key )
	if ( item == null )
		return
	expect DeathBoxListPanelItem(item)
	UpdateItem( item )
}



void function OnLocalPlayerDamaged( float damage, vector damageOrigin, int damageType, int damageSourceId, entity attacker )
{
	
	fileLevel.specialStateSamenessKey = "player_damaged"
}




void function OpenQuickSwap( var itemButton, DeathBoxEntryData entryData, bool isAltAction )
{
	fileLevel.currentQuickSwapEntry = entryData
	fileLevel.currentQuickSwapItemButton = itemButton
	fileLevel.currentQuickSwapIsAltAction = isAltAction

	if ( IsValid( itemButton ) )
	{
		Hud_SetSelected( itemButton, true )
		if ( Hud_HasToolTipData( fileLevel.currentQuickSwapItemButton ) )
		{
			ToolTipData ttd = Hud_GetToolTipData( itemButton )
			ttd.tooltipFlags = ttd.tooltipFlags | eToolTipFlag.HIDDEN
			Hud_SetToolTipData( itemButton, ttd )
		}
	}

	foreach ( string entryDataKey, DeathBoxEntryData _ in fileLevel.deathBoxEntryDataByKey )
	{
		DeathBoxListPanelItem ornull item = DeathBoxListPanel_GetItemByKey( fileLevel.listPanel, entryDataKey )
		if ( item == null )
			continue
		expect DeathBoxListPanelItem(item)
		if ( IsValid( item.allocatedButton ) )
			HudElem_SetRuiArg( item.allocatedButton, "isDimmed", true )
	}
	HudElem_SetRuiArg( itemButton, "isDimmed", false )

	RunUIScript( "ClientToUI_SurvivalGroundList_OpenQuickSwap", itemButton )
}




void function CloseQuickSwapIfOpen()
{
	RunUIScript( "ClientToUI_SurvivalGroundList_CloseQuickSwap", fileLevel.currentQuickSwapItemButton )

	if ( IsValid( fileLevel.currentQuickSwapItemButton ) )
	{
		Hud_SetSelected( fileLevel.currentQuickSwapItemButton, false )
		if ( Hud_HasToolTipData( fileLevel.currentQuickSwapItemButton ) )
		{
			ToolTipData ttd = Hud_GetToolTipData( fileLevel.currentQuickSwapItemButton )
			ttd.tooltipFlags = ttd.tooltipFlags & ~eToolTipFlag.HIDDEN
			Hud_SetToolTipData( fileLevel.currentQuickSwapItemButton, ttd )
		}
	}

	fileLevel.currentQuickSwapEntry = null
	fileLevel.currentQuickSwapItemButton = null

	foreach ( string entryDataKey, DeathBoxEntryData entryData in fileLevel.deathBoxEntryDataByKey )
	{
		DeathBoxListPanelItem ornull item = DeathBoxListPanel_GetItemByKey( fileLevel.listPanel, entryDataKey )
		if ( item == null )
			continue
		expect DeathBoxListPanelItem(item)
		if ( IsValid( item.allocatedButton ) )
			HudElem_SetRuiArg( item.allocatedButton, "isDimmed", false )
	}
}




void function UIToClient_CloseQuickSwapIfOpen()
{
	CloseQuickSwapIfOpen()
}




void function RefreshQuickSwapIfOpen()
{
	RunUIScript( "ClientToUI_SurvivalGroundList_RefreshQuickSwap" )
}






























































void function UIToClient_SurvivalGroundList_UpdateQuickSwapItem( var button, int backpackSwapSlot )
{
	entity viewPlayer = GetLocalViewPlayer()
	var buttonRui     = Hud_GetRui( button )

	Hud_ClearToolTipData( button )

	array<ConsumableInventoryItem> predictedInventory = GetPredictedInventory()
	if ( backpackSwapSlot >= predictedInventory.len() )
	{
		RunUIScript( "ClientToUI_Tooltip_Clear", button )
		Hud_SetEnabled( button, false )
		return
	}
	ConsumableInventoryItem item = predictedInventory[backpackSwapSlot]
	LootData lootFlavor          = SURVIVAL_Loot_GetLootDataByIndex( item.type )

	Hud_SetEnabled( button, true )
	Hud_SetLocked( button, SURVIVAL_IsLootIrrelevant( viewPlayer, null, lootFlavor, eLootContext.BACKPACK ) )
	Hud_SetSelected( button, IsOrdnanceEquipped( viewPlayer, lootFlavor.ref ) )

	RuiSetImage( buttonRui, "iconImage", lootFlavor.hudIcon )
	RuiSetInt( buttonRui, "lootTier", lootFlavor.tier )
	RuiSetInt( buttonRui, "count", item.count )
	int maxCount = SURVIVAL_GetInventorySlotCountForPlayer( viewPlayer, lootFlavor )
	RuiSetInt( buttonRui, "maxCount", maxCount )
	RuiSetInt( buttonRui, "ordinaryMaxCount", lootFlavor.lootType == eLootType.AMMO ? lootFlavor.inventorySlotCount : maxCount )
	RuiSetInt( buttonRui, "numPerPip", lootFlavor.lootType == eLootType.AMMO ? lootFlavor.countPerDrop : 1 )

	ToolTipData toolTipData
	toolTipData.titleText = lootFlavor.pickupString
	toolTipData.descText = SURVIVAL_Loot_GetDesc( lootFlavor, viewPlayer )
	toolTipData.actionHint1 = Localize( "#LOOT_SWAP", Localize( lootFlavor.pickupString ) ).toupper()
	toolTipData.tooltipFlags = toolTipData.tooltipFlags | (IsPingEnabledForPlayer( viewPlayer ) ? eToolTipFlag.PING_DISSABLED : 0)
	if ( Survival_PlayerCanDrop( viewPlayer ) )
		toolTipData.actionHint2 = Localize( "#LOOT_ALT_DROP" ).toupper()
	Hud_SetToolTipData( button, toolTipData )
	RunUIScript( "ClientToUI_Tooltip_MarkForClientUpdate", button, eTooltipStyle.DEFAULT )
}




















void function UIToClient_SurvivalGroundList_OnQuickSwapItemClick( var button, int backpackSwapSlot, bool isRightClick )
{
	entity localPlayer = GetLocalClientPlayer()

	DeathBoxEntryData entryData = expect DeathBoxEntryData(fileLevel.currentQuickSwapEntry)
	entity bestLootEnt          = (entryData.lootEnts.len() > 0 && IsValid( entryData.lootEnts[0] ) ? entryData.lootEnts[0] : null)
	if ( !IsValid( bestLootEnt ) )
		return

	array<ConsumableInventoryItem> predictedInventory = GetPredictedInventory()
	if ( !predictedInventory.isvalidindex( backpackSwapSlot ) )
		return 

	ConsumableInventoryItem inventoryEntry = predictedInventory[backpackSwapSlot]
	LootData dropLootFlav                  = SURVIVAL_Loot_GetLootDataByIndex( inventoryEntry.type )

	PredictedLootActionData plad
	plad.type = eLootAction.DROP_ALL
	plad.time = Time()
	plad.lootFlavRef = dropLootFlav.ref

	plad.count = -inventoryEntry.count

	fileLevel.predictedActions.append( plad )
	fileLevel.predictedActionsDirty = true

	string itemKey = dropLootFlav.ref
	if ( !(itemKey in fileLevel.deathBoxEntryDataByKey) )
	{
		DeathBoxEntryData newEntry
		newEntry.key = itemKey
		newEntry.lootFlav = dropLootFlav
		fileLevel.deathBoxEntryDataByKey[itemKey] <- newEntry
	}

	if ( isRightClick )
	{
		
		Remote_ServerCallFunction( "ClientCallback_Sur_DropBackpackItem_Box", dropLootFlav.index, inventoryEntry.count, fileLevel.currentDeathBoxEEH )
		RefreshQuickSwapIfOpen()
	}
	else
	{
		DeathBoxListPanelItem ornull item = DeathBoxListPanel_GetItemByKey( fileLevel.listPanel, entryData.key )
		if ( item != null )
		{
			expect DeathBoxListPanelItem(item)
			bool isAltAction       = fileLevel.currentQuickSwapIsAltAction
			bool isFromExtendedUse = false
			bool isFromQuickSwap   = true
			PerformItemAction( item, isAltAction, isFromExtendedUse, isFromQuickSwap, backpackSwapSlot )
		}
	}
}



































void function UIToClient_SurvivalGroundList_OnMouseLeftPressed()
{
	if ( fileLevel.currentDeathBoxEEH == EncodedEHandle_null )
		return

	var focus = GetFocus()
	if ( !IsValid( focus ) )
		return

	if ( focus != fileLevel.currentQuickSwapItemButton && !IsCursorInElementBounds( fileLevel.currentQuickSwapItemButton )
	&& focus != fileLevel.quickSwapBacker && !IsCursorInElementBounds( fileLevel.quickSwapBacker )
	&& focus != fileLevel.quickSwapGrid && !IsCursorInElementBounds( fileLevel.quickSwapGrid )
	&& Hud_GetParent( focus ) != fileLevel.quickSwapGrid
	&& focus != fileLevel.quickSwapHeader && !IsCursorInElementBounds( fileLevel.quickSwapHeader )
	&& focus != fileLevel.inventorySwapIcon && !IsCursorInElementBounds( fileLevel.inventorySwapIcon ) )
	{
		CloseQuickSwapIfOpen()
	}
}











void function Delayed_SetCursorToObject( var obj )
{
	RegisterSignal( "Delayed_SetCursorToObject" )
	Signal( clGlobal.signalDummy, "Delayed_SetCursorToObject" )
	EndSignal( clGlobal.signalDummy, "Delayed_SetCursorToObject" )

	wait 0.1 

	float width  = 1920
	float height = 1080

	UISize screenSize = GetScreenSize()
	float x           = float( Hud_GetAbsX( obj ) + Hud_GetWidth( obj ) / 2 ) / screenSize.width
	float y           = float( Hud_GetAbsY( obj ) + Hud_GetHeight( obj ) / 2 ) / screenSize.height

	SetCursorPosition( <width * x, height * y, 0> )
}




bool function IsCursorInElementBounds( var element )
{
	if ( !IsValid( element ) )
		return false

	vector cursorPos  = GetCursorPosition()
	UISize screenSize = GetScreenSize()
	cursorPos.x *= screenSize.width / 1920.0
	cursorPos.y *= screenSize.height / 1080.0

	UISize elementSize = REPLACEHud_GetSize( element )
	UIPos elementPos   = REPLACEHud_GetAbsPos( element )

	return PointInBounds( cursorPos, elementPos, elementSize )
}



