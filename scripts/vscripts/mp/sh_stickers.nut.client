global function ShStickers_LevelInit
global function Loadout_Sticker


global function GetStickerObjectType
global function GetStickerObjectModel


global function GetMaxStickersForObjectType







global function Sticker_IsTheEmpty
global function Sticker_GetStoryBlurbBodyText
global function Sticker_HasStoryBlurb
global function Sticker_GetDecalMaterialAsset
global function Sticker_GetReplacementMaterialAsset
global function Sticker_GetSortOrdinal
global function Sticker_GetDecalScale






global function Sticker_SetMaterialModForLocalPlayer
global function Sticker_PlaceDecalForLocalPlayer
global function Sticker_OnPlaced
global function Sticker_CreateFlashData
global function Sticker_FlashOnLoadComplete






#if DEV
global function DEV_TestCreateStickerMesh
global function DEV_TestCreateStickerDecal
global function DEV_StickerTestSetupForLocalPlayer
#endif





global enum eStickerObjectType
{
	injector,
	shield_cell,
	shield_battery,
	phoenix_kit,
}


global const asset UNAPPLIED_STICKER_MODEL = $"mdl/props/stickers/flat_sticker.rmdl"

const asset SHIELD_CELL_MODEL = $"mdl/weapons/shield_battery/ptpov_shield_battery_small_held.rmdl"
const asset SHIELD_BATTERY_MODEL = $"mdl/weapons/shield_battery/ptpov_shield_battery_held.rmdl"
const asset HEALTH_INJECTOR_MODEL = $"mdl/weapons/health_injector/ptpov_health_injector.rmdl"


struct StickerFlashData
{
	entity flashEnt
	int    flashType
	vector flashColor
}

struct FileStruct_LifetimeLevel
{
	table<int, array<LoadoutEntry> > stickerSlotMap
	table<ItemFlavor, int>           stickerSortOrdinalMap

	table<int, StickerFlashData> stickerFlashData
}
FileStruct_LifetimeLevel& fileLevel


void function ShStickers_LevelInit()
{
	FileStruct_LifetimeLevel newFileLevel
	fileLevel = newFileLevel


		PrecacheModel( UNAPPLIED_STICKER_MODEL )

		PrecacheModel( SHIELD_CELL_MODEL )
		PrecacheModel( SHIELD_BATTERY_MODEL )
		PrecacheModel( HEALTH_INJECTOR_MODEL )


	AddCallback_RegisterRootItemFlavors( RegisterStickers )


		DEV_SetupStickerNetworking()



	thread AutoLoadViewPlayerStickers()

}


void function RegisterStickers()
{
	array<ItemFlavor> stickerItemList = []
	foreach ( asset stickerAsset in GetBaseItemFlavorsFromArray( "stickers" ) )
	{
		if ( stickerAsset == $"" )
			continue

		ItemFlavor ornull stickerItem = RegisterItemFlavorFromSettingsAsset( stickerAsset )
		if ( stickerItem == null )
			continue

		expect ItemFlavor( stickerItem )
		stickerItemList.append( stickerItem )
	}

	MakeItemFlavorSet( stickerItemList, fileLevel.stickerSortOrdinalMap )

	foreach ( int stickerObjectType in eStickerObjectType )
	{
		fileLevel.stickerSlotMap[stickerObjectType] <- []
		string stickerObjectName = GetEnumString( "eStickerObjectType", stickerObjectType )

		for ( int i = 0; i < GetMaxStickersForObjectType( stickerObjectType ); i++ )
		{
			LoadoutEntry entry = RegisterLoadoutSlot( eLoadoutEntryType.ITEM_FLAVOR, stickerObjectName + "_sticker_" + i, eLoadoutEntryClass.ACCOUNT )
			entry.category     = eLoadoutCategory.STICKERS
#if DEV
				entry.DEV_name       = "Sticker " + stickerObjectName + " " + i
#endif
			entry.defaultItemFlavor   = stickerItemList[0]
			entry.validItemFlavorList = stickerItemList
			entry.isSlotLocked        = bool function( EHI playerEHI ) { return !IsLobby()	}
			entry.networkTo           = eLoadoutNetworking.PLAYER_EXCLUSIVE

			fileLevel.stickerSlotMap[stickerObjectType].append( entry )

			
			










		}
	}

	

	foreach ( int stickerObjectType in eStickerObjectType )
	{
		foreach( LoadoutEntry entry in fileLevel.stickerSlotMap[stickerObjectType] )
		{
			AddCallback_ItemFlavorLoadoutSlotDidChange_AnyPlayer( entry, OnStickerLocalPlayerItemLoadoutChanged )
		}
	}

}


void function OnStickerLocalPlayerItemLoadoutChanged( EHI playerEHI, ItemFlavor flavor )
{
	if ( !IsLocalClientEHIValid() )
		return

	if ( LocalClientEHI() != playerEHI )
		return

	if ( !Sticker_IsTheEmpty( flavor ) )
	{
		asset stickerAsset = Sticker_GetDecalMaterialAsset( flavor )
		RequestLoadStickerPak( stickerAsset )
	}
}



void function AutoLoadViewPlayerStickers()
{
	entity currentPlayer = GetLocalViewPlayer()

	while(true)
	{
		
		if ( currentPlayer != GetLocalViewPlayer() )
		{
			currentPlayer = GetLocalViewPlayer()

			foreach ( int stickerObjectType in eStickerObjectType )
			{
				int maxStickersForObjectType = GetMaxStickersForObjectType( stickerObjectType )

				for ( int i = 0; i < maxStickersForObjectType; i++ )
				{
					int stickerSlot = stickerObjectType * maxStickersForObjectType + i

					SettingsAssetGUID stickerLoadoutSlotGuid = currentPlayer.GetStickerSlot( stickerSlot )
					ItemFlavor ornull stickerItemOrNull = GetItemFlavorOrNullByGUID( stickerLoadoutSlotGuid )

					if (stickerItemOrNull == null)
						continue

					ItemFlavor stickerItem = expect ItemFlavor( stickerItemOrNull )

					if ( !Sticker_IsTheEmpty( stickerItem ) )
						RequestLoadStickerPak( Sticker_GetDecalMaterialAsset( stickerItem ) )
				}
			}
		}

		WaitFrame()
	}
}



int function GetStickerObjectType( string modName )
{
	switch ( modName )
	{
		case "shield_small":
			return eStickerObjectType.shield_cell

		case "shield_large":
			return eStickerObjectType.shield_battery

		case "phoenix_kit":
			return eStickerObjectType.phoenix_kit

		case "health_small":
		case "health_large":
			return eStickerObjectType.injector
	}

	return -1
}


asset function GetStickerObjectModel( int stickerObjectType )
{
	switch ( stickerObjectType )
	{
		case eStickerObjectType.shield_cell:
			return SHIELD_CELL_MODEL

		case eStickerObjectType.shield_battery:
		case eStickerObjectType.phoenix_kit:
			return SHIELD_BATTERY_MODEL

		case eStickerObjectType.injector:
			return HEALTH_INJECTOR_MODEL
	}

	Assert( false, "Unsupported stickerObjectType value " + stickerObjectType + " passed to GetStickerObjectModel()" )
	unreachable
}


int function GetMaxStickersForObjectType( int stickerObjectType )
{
	if ( stickerObjectType == eStickerObjectType.injector )
		return 1

	return 3
}
































































LoadoutEntry function Loadout_Sticker( int stickerObjectType, int index )
{
	return fileLevel.stickerSlotMap[stickerObjectType][index]
}

bool function Sticker_HasStoryBlurb( ItemFlavor flavor )
{
	Assert( ItemFlavor_GetType( flavor ) == eItemType.sticker )

	return ( Sticker_GetStoryBlurbBodyText( flavor ) != "" )
}

string function Sticker_GetStoryBlurbBodyText( ItemFlavor flavor )
{
	Assert( ItemFlavor_GetType( flavor ) == eItemType.sticker )

	return GetGlobalSettingsString( ItemFlavor_GetAsset( flavor ), "customSkinMenuBlurb" )
}

bool function Sticker_IsTheEmpty( ItemFlavor stickerItem )
{
	Assert( ItemFlavor_GetType( stickerItem ) == eItemType.sticker )

	return GetGlobalSettingsBool( ItemFlavor_GetAsset( stickerItem ), "isTheEmpty" )
}


asset function Sticker_GetDecalMaterialAsset( ItemFlavor stickerItem )
{
	Assert( ItemFlavor_GetType( stickerItem ) == eItemType.sticker )

	return GetKeyValueAsAsset( { kn = GetGlobalSettingsAsset( ItemFlavor_GetAsset( stickerItem ), "stickerMaterial" ) + "_rgdu.rpak" }, "kn" );
}


asset function Sticker_GetReplacementMaterialAsset( ItemFlavor stickerItem )
{
	Assert( ItemFlavor_GetType( stickerItem ) == eItemType.sticker )

	return GetKeyValueAsAsset( { kn = GetGlobalSettingsAsset( ItemFlavor_GetAsset( stickerItem ), "nonDecalStickerMaterial" ) + "_rgdp.rpak" }, "kn" );
}

int function Sticker_GetSortOrdinal( ItemFlavor stickerItem )
{
	Assert( ItemFlavor_GetType( stickerItem ) == eItemType.sticker )

	return fileLevel.stickerSortOrdinalMap[stickerItem]
}

float function Sticker_GetDecalScale( ItemFlavor stickerItem, int stickerObjectType, int targetSlot )
{
	Assert( ItemFlavor_GetType( stickerItem ) == eItemType.sticker )

	string defaultKey = ""
	string arrayKey = ""
	switch ( stickerObjectType )
	{
		case eStickerObjectType.shield_cell:
			defaultKey = "shieldCellScale"
			arrayKey = "shieldCellScales"
			break

		case eStickerObjectType.shield_battery:
			defaultKey = "shieldBatteryScale"
			arrayKey = "shieldBatteryScales"
			break

		case eStickerObjectType.phoenix_kit:
			defaultKey = "phoenixKitScale"
			arrayKey = "phoenixKitScales"
			break

		case eStickerObjectType.injector:
			defaultKey = "injectorScale"
			arrayKey = "injectorScales"
			break

		default:
			return 0
	}

	asset stickerAsset = ItemFlavor_GetAsset( stickerItem )

	foreach ( var block in IterateSettingsAssetArray( stickerAsset, arrayKey ) )
	{
		int slot = GetSettingsBlockInt( block, "slot" )
		if ( slot == targetSlot )
			return GetSettingsBlockFloat( block , "scale" )
	}

	return GetGlobalSettingsFloat( stickerAsset, defaultKey )
}























void function DEV_SetupStickerNetworking() 
{
	Remote_RegisterServerFunction( "DEV_StickerTestSetupForPlayer" )
}


















#if DEV
void function DEV_TestCreateStickerMesh( asset stickerMat )
{
	entity player = GP()
	vector origin = player.GetOrigin()
	vector angles = <0, 0, 0>

	entity model = CreateClientSidePropDynamic( origin, angles, UNAPPLIED_STICKER_MODEL )
	Sticker_SetMaterialModForLocalPlayer( model, stickerMat )
}

void function DEV_TestCreateStickerDecal( asset stickerMat, float scale )
{
	asset test_model = $"mdl/weapons/shield_battery/ptpov_shield_battery_held.rmdl"

	entity player = GP()
	vector origin = player.GetOrigin()
	vector angles = <0, 0, 0>

	entity model = CreateClientSidePropDynamic( origin, angles, test_model )
	Sticker_PlaceDecalForLocalPlayer( model, stickerMat, "STICKER_1", scale )
}

void function DEV_StickerTestSetupForLocalPlayer()
{
	Remote_ServerCallFunction( "DEV_StickerTestSetupForPlayer" )
}
#endif




int function Sticker_SetMaterialModForLocalPlayer( entity ent, asset stickerMat )
{
	int stickerInstance = SetEntMaterialToSticker( stickerMat, ent )
	

	return stickerInstance
}



int function Sticker_PlaceDecalForLocalPlayer( entity ent, asset stickerMat, string attachment, float scale )
{
	int stickerInstance = AddStickerDecalToEntity( ent, stickerMat, attachment, scale )
	

	return stickerInstance
}


void function Sticker_OnPlaced( int stickerInstance, void functionref( int ) callbackFunc )
{
	thread function() : ( stickerInstance, callbackFunc )
	{
		while ( IsValidStickerInstance( stickerInstance ) && !IsStickerInstancePlaced( stickerInstance ) )
			WaitFrame()

		if ( IsValidStickerInstance( stickerInstance ) )
			callbackFunc( stickerInstance )
	}()
}

void function Sticker_CreateFlashData( int stickerInstance, entity flashEnt, int flashType, vector flashColor )
{
	Sticker_CleanUpFlashData()

	
	Assert( !( stickerInstance in fileLevel.stickerFlashData ) )

	if ( !( stickerInstance in fileLevel.stickerFlashData ) )
	{
		StickerFlashData sfd
		sfd.flashEnt = flashEnt
		sfd.flashType = flashType
		sfd.flashColor = flashColor

		
		fileLevel.stickerFlashData[ stickerInstance ] <- sfd
	}
}

void function Sticker_CleanUpFlashData()
{
	array<int> deleteList
	foreach ( int stickerInstance, StickerFlashData sdf in fileLevel.stickerFlashData )
	{
		if ( !IsValid( sdf.flashEnt ) || !IsValidStickerInstance( stickerInstance ) )
			deleteList.append( stickerInstance )
	}

	foreach ( int stickerInstance in deleteList )
	{
		
		delete fileLevel.stickerFlashData[ stickerInstance ]
	}
}

void function Sticker_FlashOnLoadComplete( int stickerInstance )
{
	if ( !( stickerInstance in fileLevel.stickerFlashData ) )
		return

	entity flashEnt = fileLevel.stickerFlashData[ stickerInstance ].flashEnt

	if ( IsValid( flashEnt ) && IsValidStickerInstance( stickerInstance ) && IsStickerInstancePlaced( stickerInstance ) )
	{
		int flashType        = fileLevel.stickerFlashData[ stickerInstance ].flashType
		vector flashColor    = fileLevel.stickerFlashData[ stickerInstance ].flashColor
		bool includeChildren = false
		bool depthDiscard    = true
		thread FlashMenuModel( flashEnt, flashType, flashColor, includeChildren, depthDiscard )
	}
}
























