global function ShArtifacts_LevelInit

global function RegisterArtifactComponentsForWeapon
global function Artifacts_GetConfigurationFramework
global function Artifacts_GetAssociatedWeaponForComponent
global function Artifacts_GetComponentType
global function Artifacts_GetSetKey
global function Artifacts_GetSetIndex
global function Artifacts_GetComponentChangeGUID
global function Artifacts_IsEmptyComponent
global function Artifacts_GetComponentIcon
global function Artifacts_GetComponentMainColor
global function Artifacts_GetComponentSecondaryColor
global function Artifacts_IsItemFlavorArtifact


global function Artifacts_PlayerHasArtifactActive
global function Artifacts_StoreLoadoutDataOnPlayerEntityStruct
global function Artifacts_OnWeaponOwnerChanged
global function Artifact_PrecacheDeathboxModelAndFX 



global function Artifacts_Loadouts_GetMeleeSkinNetVarOverrideType
global function Artifacts_Loadouts_IsAnyArtifactUnlocked
global function Artifacts_Loadouts_GetConfigIndex
global function Artifacts_Loadouts_GetEntryForConfigIndexAndType
global function Artifacts_Loadouts_IsConfigPointerItemFlavor
global function Artifacts_Loadouts_IsConfigPointerGUID
global function Artifacts_Loadouts_GetEquippedTier
global function Artifacts_Loadouts_CheckAndFixMisconfigurations
global function Artifacts_Loadouts_ComponentChangeSlot

global function Artifacts_Loadouts_SetupWeaponComponents
global function Artifacts_Loadouts_ApplyModelForSet
global function Artifacts_Loadouts_SetBaseSkinForTheme
global function Artifacts_Loadouts_SetBladeMod
global function Artifacts_Loadouts_SetPowerSourceMod


global function Artifacts_Loadouts_StorePreviewDataOnPlayerEntityStruct
global function Artifacts_ClearMenuEffects
global function Artifacts_Loadouts_PreviewBladeAndPowerSource
global function Artifacts_Loadouts_PreviewTheme








global function ClientCodeCallback_GetArtifactViewmodelDataForWeapon









global function Artifacts_FX_StartWeaponFX
global function Artifacts_FX_StopWeaponFX
global function Artifacts_FX_ScriptAnimWindowCallback

global function Artifacts_FX_GetIdleFX
global function Artifacts_FX_GetFlourishFX
global function Artifacts_FX_GetAttackFX
global function Artifacts_FX_GetStartupFX
global function Artifacts_FX_GetLobbyFX
global function Artifacts_FX_GetEmissiveAndSmearColor
global function Artifacts_FX_GetSkinIndex
global function Artifacts_FX_Get1PAnd3PFXArraysForWeapon

global function Artifacts_FX_GetDeathboxFX
global function Artifacts_GetDeathboxModel
global function Artifacts_FX_GetDeathboxSpawnSFX

global function Artifacts_FX_GetBladeControlPoints


















global function Artifacts_GetSetItems
global function Artifacts_IsBaseArtifact
global function Artifacts_IsBaseArtifactOwned
global function Artifacts_ActivationEmote_GetVideo


#if DEV













#endif

global enum eArtifactComponentType {
	BLADE,
	THEME,
	POWER_SOURCE,
	DEATHBOX,
	ACTIVATION_EMOTE,

	COUNT,
}

global enum eArtifactConfigurationType {
	SKIN_ONLY,
	COMPONENT,

	COUNT,
}

global enum eArtifactFXPackageType {
	INVALID = -1,
	ATTACK,
	IDLE,
	STARTUP,
	FLOURISH,
	INSPECT,
	LOBBY,

	COUNT,
	
	
	BLADE_EMISSIVE,
}


global struct ArtifactFXPackage {
	int fxPackageType = eArtifactFXPackageType.INVALID 
	asset fxAsset = $""
	string attachName = ""
	bool is1P = false 

	int attachType = FX_PATTACH_POINT_FOLLOW
}

global struct ArtifactFX1PAnd3P {
	array< ArtifactFXPackage > fx1P
	array< ArtifactFXPackage > fx3P
}

global struct ArtifactFXControlPoint {
	vector controlPoint
	int controlPointNumber
	string controlPointName
}


global struct ArtifactConfig
{
	ItemFlavor& character
	ItemFlavor& powerSource
	ItemFlavor& theme
	ItemFlavor& blade
	ItemFlavor& deathbox
	ItemFlavor& activationEmote
}

struct ArtifactThemeModelData
{
	asset worldModel
	asset viewModel
}









struct FileStruct_LifetimeLevel
{
	ItemFlavor& artifactWeapon 

	array< ItemFlavor > allComponents
	array< array< ItemFlavor > > componentListsByType 

	array< array< LoadoutEntry > > loadoutConfigurationEntriesByIndexAndType
	table< LoadoutEntry, int > loadoutConfigurationSlotsToIndices

	LoadoutEntry& componentChangeSlot

	
	table< int, array< ItemFlavor > > componentSets 
	table< int, ArtifactThemeModelData > setModels 











	array< int > lobbyFxHandles

}
FileStruct_LifetimeLevel& fileLevel

global const string ARTIFACT_DAGGER_MP_WEAPON = "mp_weapon_artifact_dagger_primary"
global const string ARTIFACT_DAGGER_MELEE_WEAPON = "melee_artifact_dagger"

global const int ARTIFACT_FX_HANDLE_INVALID = -1
global const string ARTIFACT_POWER_SOURCE_MODIFIER_SUFFIX = "_ps"

global const int ARTIFACT_CONFIGURATION_PTR_0_GUID = 1182724979

const int ARTIFACT_DAGGER_ITEM_FLAVOR_GUID = 2113589622

const float DEACTIVATED_EMISSIVE_FACTOR = 0.15
const float ACTIVATION_EMOTE_ANIM_TIME_RESTART_LIMIT = 2.0 

const string WORLD_MODEL = "worldModel"
const string VIEW_MODEL = "viewModel"
const string SET_NAME = "setName"
const string SET_IMAGE_REF = "setImageRef"

const string BASE_WEAPON = "base_weapon"
const table< int, string > ARTIFACT_COMPONENTS_TO_LOADOUT_NAMES_MAP = {
	[eArtifactComponentType.BLADE] = "blade",
	[eArtifactComponentType.THEME] = "theme",
	[eArtifactComponentType.POWER_SOURCE] = "power_source",
	[eArtifactComponentType.DEATHBOX] = "deathbox",
	[eArtifactComponentType.ACTIVATION_EMOTE] = "activation_emote",
}


const string EMPTY = "_EMPTY"
const string RAGOLD = "RAGOLD"
global enum eArtifactSetIndex { 
	
	




	RAGOLD = -2,
	_EMPTY = -1, 
	CELES = 0,
	DEATH = 1,
	HISOC = 2,
	MOB = 3,
	STEAM = 4,
	STECH = 5,
	COUNT = 6 
}

const int LOADOUT_MELEE_SKIN_ITEM_TYPE_OVERRIDE = eItemType.artifact_component_blade
const int LOADOUT_MELEE_SKIN_COMPONENT_TYPE_OVERRIDE = eArtifactComponentType.BLADE
global const int ULTIMATE_SET_INDEX = eArtifactSetIndex.CELES
const int BASE_SET_INDEX = eArtifactSetIndex.MOB 

const string BLADE_KEY = "blade"
const string THEME_KEY = "theme"
const string POWER_SOURCE_KEY = "powerSource"
const string DEATHBOX_KEY = "deathbox"
const string ACTIVATION_EMOTE_KEY = "activationEmote"
global const table<string, int> ARTIFACT_COMPONENT_SETTINGS_KEYS = {
	[BLADE_KEY] = eArtifactComponentType.BLADE,
	[THEME_KEY] = eArtifactComponentType.THEME,
	[POWER_SOURCE_KEY] = eArtifactComponentType.POWER_SOURCE,
	[DEATHBOX_KEY] = eArtifactComponentType.DEATHBOX,
	[ACTIVATION_EMOTE_KEY] = eArtifactComponentType.ACTIVATION_EMOTE,
}



































const int ARTIFACT_MAX_LOADOUTS = 3
const string ONE_P = "1P"
const string THREE_P = "3P"
const int BODY_GROUP_INVALID = -1

const int THEME_BASE = 1 
const int THEME_SHINY = 2 

const string ARTIFACT_EMISSIVE_FX_SIGNAL = "ArtifactsEmissiveFxSignal"


const string LOADOUTS_ARTIFACT_INDEX_COMPONENT_TYPE = "artifact_%d_component_%s"
const string LOADOUTS_ARTIFACT_COMPONENT_CHANGE = "artifact_configuration_component_change"
#if DEV
const string LOADOUTS_DEV_ARTIFACT_COMPONENT_CHANGE_VERBOSE_PDEF_SECTION_KEY = "artifact configuration component change"
const string LOADOUTS_DEV_ARTIFACT_COMPONENT_CHANGE_VERBOSE = "Artifact Configuration Componenent Change"
const string LOADOUTS_DEV_ARTIFACT_CONFIGURATION_PDEF_SECTION_KEY = "artifact configuration %d"
const string LOADOUTS_DEV_ARTIFACT_CONFIGURATION_VERBOSE = "Artifact Configuration %d"
#endif


const string COMPONENT_TYPE_SETTINGS_BLOCK_KEY = "artifactComponentType"
const string CONFIGURATION_TYPE_SETTINGS_BLOCK_KEY = "configurationFramework"
const string CONFIG_POINTER_INDEX_KEY = "artifactConfigIndex"
const string IS_CONFIG_POINTER_KEY = "isArtifactConfigPointer"
const string IS_EMPTY = "isEmpty" 
const string THEME_NAME = "themeName" 
const string COMPONENT_SETS = "componentSets"
const string SET_ASSET = "set"
const string BLADE_BODY_GROUP_MOD_PREFIX = "blade" 
const string POWER_SOURCE_BODY_GROUP_MOD_PREFIX = "power" 

const string SCRIPT_ANIM_WINDOW_FX = "scriptAnimWindowFXControls"
const string SCRIPT_ANIM_WINDOW_PARAMETER = "parameter"
const string SCRIPT_ANIM_WINDOW_SET_FX = "setSpecificFX"
const string SCRIPT_ANIM_WINDOW_UNIVERSAL = "UNIVERSAL" 
const string SCRIPT_ANIM_WINDOW_FX_PACKAGE_TYPE = "fxPackageType"
const string SCRIPT_ANIM_WINDOW_ACTION = "action"
const string SCRIPT_ANIM_WINDOW_STOP = "STOP"
const string SCRIPT_ANIM_WINDOW_START = "START"

const string DEATHBOX_FX_NAME_KEY = "artifactDeathboxFXName"
const string DEATHBOX_MDL_REF_KEY = "artifactDeathboxModel"
const string DEATHBOX_SFX_SPAWN_KEY = "spawnSFX"

const string FX_SMEAR_COLOR = "smearColor"
const string FX_SKIN_INDEX = "skinIdx"
const string FX_ASSET = "fxAsset"
const string FX_ATTACH = "attachName"
const string FX_PERSPECTIVE = "perspective"
const array<string> FX_FIELDS = [
	FX_ASSET,
	FX_ATTACH,
	FX_PERSPECTIVE,
]
const string FX_PACKAGE_FLOURISH = "flourishFX"
const string FX_PACKAGE_ATTACK = "attackFX"
const string FX_PACKAGE_IDLE = "idleFX"
const string FX_PACKAGE_STARTUP = "startupFX"
const string FX_PACKAGE_INSPECT = "inspectFX"
const string FX_PACKAGE_LOBBY = "lobbyFX"
const array<string> FX_PACKAGES = [
	FX_PACKAGE_FLOURISH,
	FX_PACKAGE_ATTACK,
	FX_PACKAGE_IDLE,
	FX_PACKAGE_STARTUP,
	FX_PACKAGE_INSPECT,
	FX_PACKAGE_LOBBY,
]

const string FX_CONTROL_POINT_LIST = "fxControlPoints"
const string FX_CONTROL_POINT_LIST_LOBBY = "lobbyFxControlPoints"
const string FX_CONTROL_POINT = "controlPoint"
const string FX_CONTROL_POINT_NUMBER = "controlPointNumber"
const string FX_CONTROL_POINT_NAME = "controlPointName"
const array<string> FX_CONTROL_POINT_PROPERTIES = [
	FX_CONTROL_POINT,
	FX_CONTROL_POINT_NUMBER,
	FX_CONTROL_POINT_NAME,
]
const vector FX_NULL_CAP_EMISSIVE = <0, 0, 0>



const asset VFX_TEST_DEATHBOX_PARTICLE = $"P_death_box_mob_kill_fx"
const asset VFX_TEST_IDLE = $"P_car_reac_spinners_lvl4"

const asset VFX_MOB_STARTUP_1P = $"P_art_MOB_power_start_FP"
const asset VFX_MOB_STARTUP_3P = $"P_art_MOB_power_start_3P"
const asset VFX_MOB_IDLE_1P = $"P_art_MOB_power_idle_FP"
const asset VFX_MOB_IDLE_3P = $"P_art_MOB_power_idle_3P"
const asset VFX_MOB_IDLE_BLADE_1P = $"P_art_MOB_blade_idle_FP"
const asset VFX_MOB_IDLE_BLADE_3P = $"P_art_MOB_blade_idle_3P"
const asset VFX_MOB_ATTACK_1P = $"P_art_MOB_blade_attack_FP"
const asset VFX_MOB_ATTACK_1P_CP = $"P_art_MOB_blade_attack_FP_CP"
const asset VFX_MOB_ATTACK_3P = $"P_art_MOB_blade_attack_3P"
const asset VFX_MOB_FLOURISH_1P = $"P_art_MOB_blade_flourish_FP"
const asset VFX_MOB_FLOURISH_3P = $"P_art_MOB_blade_flourish_3P"
const asset VFX_MOB_LOBBY_BLADE = $"P_art_MOB_blade_idle_Menu_CP" 
const asset VFX_MOB_INSPECT_1P = $"P_art_MOB_power_inspect_FP"
const asset VFX_MOB_LOBBY_POWER_SOURCE = $"P_art_MOB_power_idle_Menu"

const asset VFX_celes_STARTUP_1P = $"P_art_celes_power_start_FP"
const asset VFX_celes_STARTUP_3P = $"P_art_celes_power_start_3P"
const asset VFX_celes_IDLE_1P = $"P_art_celes_power_idle_FP"
const asset VFX_celes_IDLE_3P = $"P_art_celes_power_idle_3P"
const asset VFX_celes_IDLE_BLADE_1P = $"P_art_celes_blade_idle_FP"
const asset VFX_celes_IDLE_BLADE_3P = $"P_art_celes_blade_idle_3P"
const asset VFX_celes_ATTACK_1P = $"P_art_celes_power_attack_FP"
const asset VFX_celes_ATTACK_3P = $"P_art_celes_power_attack_3P"
const asset VFX_celes_ATTACK_BLADE_1P = $"P_art_celes_blade_attack_FP"
const asset VFX_celes_ATTACK_BLADE_3P = $"P_art_celes_blade_attack_3P"
const asset VFX_celes_FLOURISH_1P = $"P_art_celes_blade_flourish_FP"
const asset VFX_celes_FLOURISH_3P = $"P_art_celes_blade_flourish_3P"
const asset VFX_celes_INSPECT_1P = $"P_art_celes_power_inspect_FP"
const asset VFX_celes_DEATHBOX = $"P_death_box_artifact_celes"

const asset VFX_death_STARTUP_1P = $"P_art_death_power_start_FP"
const asset VFX_death_STARTUP_3P = $"P_art_death_power_start_3P"
const asset VFX_death_IDLE_1P = $"P_art_death_power_idle_FP"
const asset VFX_death_IDLE_3P = $"P_art_death_power_idle_3P"
const asset VFX_death_IDLE_BLADE_1P = $"P_art_death_blade_idle_FP"
const asset VFX_death_IDLE_BLADE_3P = $"P_art_death_blade_idle_3P"
const asset VFX_death_ATTACK_1P = $"P_art_death_blade_attack_FP"
const asset VFX_death_ATTACK_3P = $"P_art_death_blade_attack_3P"
const asset VFX_death_FLOURISH_1P = $"P_art_death_blade_flourish_FP"
const asset VFX_death_FLOURISH_3P = $"P_art_death_blade_flourish_3P"
const asset VFX_death_INSPECT_1P = $"P_art_death_power_idle_FP"
const asset VFX_death_DEATHBOX = $"P_death_box_artifact_death_mdl"

const asset VFX_hisoc_STARTUP_1P = $"P_art_hisoc_power_start_FP"
const asset VFX_hisoc_STARTUP_3P = $"P_art_hisoc_power_start_3P"
const asset VFX_hisoc_IDLE_1P = $"P_art_hisoc_power_idle_FP"
const asset VFX_hisoc_IDLE_3P = $"P_art_hisoc_power_idle_3P"
const asset VFX_hisoc_IDLE_BLADE_1P = $"P_art_hisoc_blade_idle_FP"
const asset VFX_hisoc_IDLE_BLADE_3P = $"P_art_hisoc_blade_idle_3P"
const asset VFX_hisoc_ATTACK_1P = $"P_art_hisoc_blade_attack_FP"
const asset VFX_hisoc_ATTACK_3P = $"P_art_hisoc_blade_attack_3P"
const asset VFX_hisoc_FLOURISH_1P = $"P_art_hisoc_blade_flourish_FP"
const asset VFX_hisoc_FLOURISH_3P = $"P_art_hisoc_blade_flourish_3P"
const asset VFX_hisoc_INSPECT_1P = $"P_art_hisoc_power_inspect_FP"

const asset VFX_steam_STARTUP_1P = $"P_art_steam_power_start_FP"
const asset VFX_steam_STARTUP_3P = $"P_art_steam_power_start_3P"
const asset VFX_steam_IDLE_1P = $"P_art_steam_power_idle_FP"
const asset VFX_steam_IDLE_3P = $"P_art_steam_power_idle_3P"
const asset VFX_steam_IDLE_BLADE_1P = $"P_art_steam_blade_idle_FP"
const asset VFX_steam_IDLE_BLADE_3P = $"P_art_steam_blade_idle_3P"
const asset VFX_steam_ATTACK_1P = $"P_art_steam_blade_attack_FP"
const asset VFX_steam_ATTACK_3P = $"P_art_steam_blade_attack_3P"
const asset VFX_steam_FLOURISH_1P = $"P_art_steam_blade_flourish_FP"
const asset VFX_steam_FLOURISH_3P = $"P_art_steam_blade_flourish_3P"
const asset VFX_steam_INSPECT_1P = $"P_art_steam_power_inspect_FP"

const asset VFX_stech_STARTUP_1P = $"P_art_stech_power_start_FP"
const asset VFX_stech_STARTUP_3P = $"P_art_stech_power_start_3P"
const asset VFX_stech_IDLE_1P = $"P_art_stech_power_idle_FP"
const asset VFX_stech_IDLE_3P = $"P_art_stech_power_idle_3P"
const asset VFX_stech_IDLE_BLADE_1P = $"P_art_stech_blade_idle_FP"
const asset VFX_stech_IDLE_BLADE_3P = $"P_art_stech_blade_idle_3P"
const asset VFX_stech_ATTACK_1P = $"P_art_stech_blade_attack_FP"
const asset VFX_stech_ATTACK_3P = $"P_art_stech_blade_attack_3P"
const asset VFX_stech_FLOURISH_1P = $"P_art_stech_blade_flourish_FP"
const asset VFX_stech_FLOURISH_3P = $"P_art_stech_blade_flourish_3P"
const asset VFX_stech_INSPECT_1P = $"P_art_stech_power_inspect_FP"

const asset VFX_MOB_IDLE_BLADE_1P_NO_CP = $"P_art_MOB_blade_idle_FP"
const asset VFX_MOB_IDLE_BLADE_1P_CP = $"P_art_MOB_blade_idle_FP_CP"





void function ShArtifacts_LevelInit()
{
	FileStruct_LifetimeLevel newFileLevel
	fileLevel = newFileLevel

	
	AddCallback_OnPreAllItemFlavorsRegistered( OnAllItemFlavorsRegistered_Artifact_Weapon )

	


	
	Remote_RegisterServerFunction( "ClientCallback_ActivationEmote", "bool" )







	RegisterSignal( ARTIFACT_EMISSIVE_FX_SIGNAL )
}

void function RegisterArtifactComponentsForWeapon( ItemFlavor artifactWeapon )
{
#if DEV
		printf( "ArtifactsDebug: %s", FUNC_NAME() )
#endif

	Assert( !IsValidItemFlavorGUID( fileLevel.artifactWeapon.guid ) )
	fileLevel.artifactWeapon = artifactWeapon

	fileLevel.componentListsByType.clear()
	fileLevel.componentListsByType.resize( eArtifactComponentType.COUNT )

	var settingsBlock = ItemFlavor_GetSettingsBlock( artifactWeapon )
	foreach ( var componentBlock in IterateSettingsArray( GetSettingsBlockArray( settingsBlock, COMPONENT_SETS ) ) )
	{
		asset componentSet                  = GetSettingsBlockAsset( componentBlock, SET_ASSET )
		string currentTheme                 = ""
		array< ItemFlavor > componentsInSet = []
		componentsInSet.resize( eArtifactComponentType.COUNT )

		string setTheme = GetGlobalSettingsString( componentSet, THEME_NAME )
		Assert( setTheme in eArtifactSetIndex )
		int setIndex = eArtifactSetIndex[ setTheme ]

		
		if ( setIndex >= 0 )
		{
			Assert( setTheme in eArtifactSetIndex )

			asset worldModel = GetGlobalSettingsAsset( componentSet, WORLD_MODEL )
			asset viewModel  = GetGlobalSettingsAsset( componentSet, VIEW_MODEL )

			Assert( worldModel != $"" && viewModel != $"" )

			ArtifactThemeModelData modelData

			modelData.worldModel = worldModel
			modelData.viewModel  = viewModel

			fileLevel.setModels[ eArtifactSetIndex[ setTheme ] ] <- modelData

			PrecacheModel( worldModel )
			PrecacheModel( viewModel )

			
			SetWeaponLegendaryModel( MeleeWeapon_GetOffhandWeaponClassname( artifactWeapon ), eArtifactSetIndex[ setTheme ], viewModel, worldModel )
			SetWeaponLegendaryModel( MeleeWeapon_GetMainWeaponClassname( artifactWeapon ), eArtifactSetIndex[ setTheme ], viewModel, worldModel )
		}


















		foreach ( string componentKey, int componentType in ARTIFACT_COMPONENT_SETTINGS_KEYS )
		{
			asset settingsAsset = GetGlobalSettingsAsset( componentSet, componentKey )
			if ( settingsAsset != $"" )
			{
				ItemFlavor ornull component = RegisterItemFlavorFromSettingsAsset( settingsAsset )
				if ( component != null )
				{
					expect ItemFlavor( component )
					fileLevel.componentListsByType[ Artifacts_GetComponentType( component ) ].append( component )
					Assert( !fileLevel.allComponents.contains( component ) )
					fileLevel.allComponents.append( component )
					Assert( componentType == Artifacts_GetComponentType( component ) )
					componentsInSet[ Artifacts_GetComponentType( component ) ] = component


					if ( Artifacts_GetComponentType( component ) == eArtifactComponentType.POWER_SOURCE )
					{
						Artifact_PrecacheImpactTables( component )
						Artifact_PrecachePowerSourceFX( component )
					}
					else if ( !IsLobby() && Artifacts_GetComponentType( component ) == eArtifactComponentType.DEATHBOX )
					{
						Artifact_PrecacheDeathboxModelAndFX( component )
					}


					string themeName = Artifacts_GetSetKey( component )
					Assert( currentTheme == "" || themeName == currentTheme )
					currentTheme = themeName

#if DEV

					switch( Artifacts_GetComponentType( component ) )
					{
						case eArtifactComponentType.POWER_SOURCE:
							Artifacts_DEV_UnitTest_PowerSource( component )
							break

						case eArtifactComponentType.DEATHBOX:
							Artifacts_DEV_UnitTest_Deathbox( component )
							break

						case eArtifactComponentType.BLADE:
							Artifacts_DEV_UnitTest_Blade( component )
							break
					}

#endif
				}
			}
			else
			{
				Assert( setIndex <= eArtifactSetIndex.RAGOLD )
				ItemFlavor invalidComponent
				componentsInSet[ componentType ] = invalidComponent
			}
		}

		fileLevel.componentSets[ setIndex ] <- componentsInSet
	}

}

void function OnAllItemFlavorsRegistered_Artifact_Weapon()
{
	BuildLoadoutEntries_ArtifactWeapons()
}


void function BuildLoadoutEntries_ArtifactWeapons()
{
#if DEV
		printf("ArtifactsDebug: %s", FUNC_NAME() )
#endif

	fileLevel.loadoutConfigurationEntriesByIndexAndType.clear()
	for ( int artifactIdx = 0; artifactIdx < ARTIFACT_MAX_LOADOUTS; artifactIdx++ )
	{
		fileLevel.loadoutConfigurationEntriesByIndexAndType.append( [] )
		for ( int componentCounter = 0; componentCounter < eArtifactComponentType.COUNT; componentCounter++ )
		{
			if ( fileLevel.componentListsByType[ componentCounter ].len() == 0 )
				continue 

#if DEV
				printf( "ArtifactsDebug: %s - %s", FUNC_NAME(), ARTIFACT_COMPONENTS_TO_LOADOUT_NAMES_MAP[ componentCounter ] )
#endif

			LoadoutEntry componentEntry = RegisterLoadoutSlot( eLoadoutEntryType.ITEM_FLAVOR, format( LOADOUTS_ARTIFACT_INDEX_COMPONENT_TYPE, artifactIdx, ARTIFACT_COMPONENTS_TO_LOADOUT_NAMES_MAP[ componentCounter ] ), eLoadoutEntryClass.ACCOUNT )
			componentEntry.category = eLoadoutCategory.ARTIFACT_CONFIGURATIONS
#if DEV
				componentEntry.pdefSectionKey = format( LOADOUTS_DEV_ARTIFACT_CONFIGURATION_PDEF_SECTION_KEY, artifactIdx )
				componentEntry.DEV_name = format( LOADOUTS_DEV_ARTIFACT_CONFIGURATION_VERBOSE, artifactIdx )
#endif

			componentEntry.defaultItemFlavor   = fileLevel.componentSets[ eArtifactSetIndex._EMPTY ][ componentCounter ] 
			componentEntry.validItemFlavorList = fileLevel.componentListsByType[ componentCounter ]

			componentEntry.isSlotLocked              = bool function( EHI playerEHI ) { return !IsLobby() }
			componentEntry.associatedCharacterOrNull = null
			componentEntry.networkTo                 = eLoadoutNetworking.PLAYER_EXCLUSIVE 
			componentEntry.stryderCharDataArrayIndex = ePlayerStryderCharDataArraySlots.INVALID

			fileLevel.loadoutConfigurationEntriesByIndexAndType[ artifactIdx ].append( componentEntry )
			fileLevel.loadoutConfigurationSlotsToIndices[ componentEntry ] <- artifactIdx
		}
	}

	
	ArtifactsCallback_AddArtifactDeathboxesToCharacterLoadouts( fileLevel.componentListsByType[ eArtifactComponentType.DEATHBOX ] )

	






	LoadoutEntry componentEntry = RegisterLoadoutSlot( eLoadoutEntryType.ITEM_FLAVOR, LOADOUTS_ARTIFACT_COMPONENT_CHANGE, eLoadoutEntryClass.ACCOUNT )
	componentEntry.category = eLoadoutCategory.ARTIFACT_CONFIGURATIONS
#if DEV
		componentEntry.pdefSectionKey = LOADOUTS_DEV_ARTIFACT_COMPONENT_CHANGE_VERBOSE_PDEF_SECTION_KEY
		componentEntry.DEV_name = LOADOUTS_DEV_ARTIFACT_COMPONENT_CHANGE_VERBOSE
#endif
	componentEntry.defaultItemFlavor   		 = fileLevel.componentSets[ eArtifactSetIndex._EMPTY ][ eArtifactComponentType.BLADE ]
	componentEntry.validItemFlavorList 		 = fileLevel.allComponents
	componentEntry.isSlotLocked              = bool function( EHI playerEHI ) { return !IsLobby() }
	componentEntry.associatedCharacterOrNull = null
	componentEntry.networkTo                 = eLoadoutNetworking.PLAYER_EXCLUSIVE
	componentEntry.stryderCharDataArrayIndex = ePlayerStryderCharDataArraySlots.INVALID
	fileLevel.componentChangeSlot = componentEntry


		RegisterBladesControlPoints()

}


void function RegisterBladesControlPoints()
{
	array< ItemFlavor > blades = fileLevel.componentListsByType[ eArtifactComponentType.BLADE ]

	foreach ( ItemFlavor blade in blades )
	{
		array< ArtifactFXControlPoint > controlPoints = Artifacts_FX_GetBladeControlPoints( blade, false )

		array< vector > controlPointPositions

		foreach ( ArtifactFXControlPoint controlPoint in controlPoints )
		{
			controlPointPositions.append( controlPoint.controlPoint )
		}

		RegisterArtifactBladeControlPoints( blade.guid, controlPointPositions )
	}
}


bool function Artifacts_IsEmptyComponent( ItemFlavor component )
{
	Assert( ItemFlavor_GetType( component ) > eItemType.artifact_component_START && ItemFlavor_GetType( component ) < eItemType.artifact_component_END )
	return ( GetGlobalSettingsBool( ItemFlavor_GetAsset( component ), IS_EMPTY ) )
}

asset function Artifacts_GetComponentIcon( ItemFlavor component )
{
	Assert( ItemFlavor_GetType( component ) > eItemType.artifact_component_START && ItemFlavor_GetType( component ) < eItemType.artifact_component_END )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( component ), "componentIcon" )
}

vector function Artifacts_GetComponentMainColor( ItemFlavor component )
{
	Assert( ItemFlavor_GetType( component ) > eItemType.artifact_component_START && ItemFlavor_GetType( component ) < eItemType.artifact_component_END )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( component ), "artifactMainColor" )
}

vector function Artifacts_GetComponentSecondaryColor( ItemFlavor component )
{
	Assert( ItemFlavor_GetType( component ) > eItemType.artifact_component_START && ItemFlavor_GetType( component ) < eItemType.artifact_component_END )
	return GetGlobalSettingsVector( ItemFlavor_GetAsset( component ), "artifactSecondaryColor" )
}

int function Artifacts_GetConfigurationFramework( ItemFlavor weapon )
{
	Assert( ItemFlavor_GetType( weapon ) == eItemType.melee_weapon )

	var weaponBlock = ItemFlavor_GetSettingsBlock( weapon )
	string configurationName = GetSettingsBlockString( weaponBlock, CONFIGURATION_TYPE_SETTINGS_BLOCK_KEY )

	Assert ( configurationName in eArtifactConfigurationType )
	return eArtifactConfigurationType[ configurationName ]
}

int function Artifacts_GetComponentType( ItemFlavor component )
{
	Assert( ItemFlavor_GetType( component ) > eItemType.artifact_component_START && ItemFlavor_GetType( component ) < eItemType.artifact_component_END )

	var componentBlock = ItemFlavor_GetSettingsBlock( component )
	string typeName = GetSettingsBlockString( componentBlock, COMPONENT_TYPE_SETTINGS_BLOCK_KEY )

	Assert ( typeName in eArtifactComponentType )
	return eArtifactComponentType[ typeName ]
}

string function Artifacts_GetSetKey( ItemFlavor component )
{
	Assert( ItemFlavor_GetType( component ) > eItemType.artifact_component_START && ItemFlavor_GetType( component ) < eItemType.artifact_component_END )
	return GetSettingsBlockString( ItemFlavor_GetSettingsBlock( component ), THEME_NAME )
}

int function Artifacts_GetSetIndex( ItemFlavor component )
{
	Assert( ItemFlavor_GetType( component ) > eItemType.artifact_component_START && ItemFlavor_GetType( component ) < eItemType.artifact_component_END )
	string themeName = GetSettingsBlockString( ItemFlavor_GetSettingsBlock( component ), THEME_NAME )
	Assert( themeName in eArtifactSetIndex )
	return eArtifactSetIndex[ themeName ]
}

bool function Artifacts_IsItemFlavorArtifact( ItemFlavor component )
{
	return ( ItemFlavor_GetType( component ) > eItemType.artifact_component_START && ItemFlavor_GetType( component ) < eItemType.artifact_component_END )
}


int function Artifacts_Loadouts_GetMeleeSkinNetVarOverrideType( bool getComponentType )
{
	return getComponentType ? LOADOUT_MELEE_SKIN_COMPONENT_TYPE_OVERRIDE : LOADOUT_MELEE_SKIN_ITEM_TYPE_OVERRIDE
}

bool function Artifacts_Loadouts_IsAnyArtifactUnlocked( EHI playerEHI, bool shouldIgnoreGRX )
{
	foreach ( ItemFlavor blade in fileLevel.componentListsByType[ eArtifactComponentType.BLADE ] )
	{
		if ( ItemFlavor_GetGRXMode( blade ) == eItemFlavorGRXMode.NONE )
			continue

		if ( IsItemFlavorGRXUnlockedForLoadoutSlot( playerEHI, blade, shouldIgnoreGRX ) )
			return true
	}

	return false
}

bool function Artifacts_Loadouts_IsConfigPointerItemFlavor( ItemFlavor flav )
{
	Assert( ItemFlavor_GetType( flav ) == eItemType.melee_skin )

	var block = ItemFlavor_GetSettingsBlock( flav )
	return GetSettingsBlockBool( block, IS_CONFIG_POINTER_KEY )
}

bool function Artifacts_Loadouts_IsConfigPointerGUID( int guid )
{
	if ( !IsValidItemFlavorGUID( guid ) )
		return false

	ItemFlavor flav = GetItemFlavorByGUID( guid )
	Assert( ItemFlavor_GetType( flav ) == eItemType.melee_skin )

	var block = ItemFlavor_GetSettingsBlock( flav )
	return GetSettingsBlockBool( block, IS_CONFIG_POINTER_KEY )
}

int function Artifacts_Loadouts_GetConfigIndex( ItemFlavor configPtr )
{
	Assert( Artifacts_Loadouts_IsConfigPointerGUID( ItemFlavor_GetGUID( configPtr ) ) )

	var block = ItemFlavor_GetSettingsBlock( configPtr )
	int configIdx = GetSettingsBlockInt( block, CONFIG_POINTER_INDEX_KEY )

	Assert( configIdx >= 0 && configIdx < ARTIFACT_MAX_LOADOUTS )

	return configIdx
}

bool function Artifacts_Loadouts_CheckAndFixMisconfigurations( EHI playerEHI, ItemFlavor character )
{
	bool isMisconfigured = false
	
	
	
	LoadoutEntry skinSlot = Loadout_MeleeSkin( character )
	ItemFlavor meleeSkin  = LoadoutSlot_GetItemFlavor( playerEHI, skinSlot )

	if ( !IsLobby() && !Artifacts_Loadouts_IsConfigPointerItemFlavor( meleeSkin ) )
		return isMisconfigured

	for ( int i = 0; i < ARTIFACT_MAX_LOADOUTS; i++ )
	{
		if ( !IsLobby() && i != Artifacts_Loadouts_GetConfigIndex( meleeSkin ) )
			continue 

		LoadoutEntry bladeSlot    = Artifacts_Loadouts_GetEntryForConfigIndexAndType( i, eArtifactComponentType.BLADE )
		ItemFlavor bladeComponent = LoadoutSlot_GetItemFlavor_ForValidation( playerEHI, bladeSlot )

		isMisconfigured = ( Artifacts_IsBaseArtifactOwned( FromEHI( playerEHI ) ) && Artifacts_IsEmptyComponent( bladeComponent ) ) || !IsItemFlavorUnlockedForLoadoutSlot( playerEHI, bladeSlot, bladeComponent )

		if ( isMisconfigured && IsLobby() )
		{
			LoadoutEntry slotToSet = skinSlot
			ItemFlavor component   = skinSlot.defaultItemFlavor
			foreach ( ItemFlavor blade in bladeSlot.validItemFlavorList )
			{
				if ( Artifacts_IsEmptyComponent( blade ) )
					continue

				if ( IsItemFlavorUnlockedForLoadoutSlot( playerEHI, bladeSlot, blade ) )
				{
					slotToSet = bladeSlot
					component = blade
					break
				}
			}




				RequestSetItemFlavorLoadoutSlot( playerEHI, slotToSet, component )

		}
	}

	return isMisconfigured
}

LoadoutEntry function Artifacts_Loadouts_ComponentChangeSlot()
{
	return fileLevel.componentChangeSlot
}


void function Artifacts_Loadouts_StorePreviewDataOnPlayerEntityStruct( int componentGUID )
{
	Assert( IsLobby() )
	Assert( IsValidItemFlavorGUID( componentGUID ) )
	ItemFlavor component = GetItemFlavorByGUID( componentGUID )

	entity player = GetLocalClientPlayer()
	ArtifactConfig artifactConfig
	if ( player.p.artifactConfig != null )
		artifactConfig = expect ArtifactConfig( player.p.artifactConfig )

	switch ( Artifacts_GetComponentType( component ) )
	{
		case eArtifactComponentType.BLADE:
			artifactConfig.blade = Artifacts_IsEmptyComponent( component ) ? fileLevel.componentSets[ BASE_SET_INDEX ][ eArtifactComponentType.BLADE ] : component
			break

		case eArtifactComponentType.THEME:
			artifactConfig.theme = component
			break

		case eArtifactComponentType.POWER_SOURCE:
			artifactConfig.powerSource = component
			break

		case eArtifactComponentType.ACTIVATION_EMOTE:
			artifactConfig.activationEmote = component
			break

		case eArtifactComponentType.DEATHBOX:
			artifactConfig.deathbox = component
			break

		default:
			Assert( false, "invalid Artifact component type" )
	}

	if ( Artifacts_IsEmptyComponent( artifactConfig.blade ) )
	{
		
		LoadoutEntry slot = Artifacts_Loadouts_GetEntryForConfigIndexAndType( 0, eArtifactComponentType.BLADE )
		artifactConfig.blade = slot.validItemFlavorList[1]
	}

	player.p.artifactConfig = artifactConfig
}



void function Artifacts_StoreLoadoutDataOnPlayerEntityStruct( entity player, entity weapon, bool forceClear )
{

		if ( player == null ) 
		{
			player = GetLocalClientPlayer()
		}
		else if ( player != GetLocalClientPlayer() )
		{
			Assert( IsValid( weapon ) && weapon.IsWeaponX() )
			if ( !IsValid( weapon ) || !weapon.IsWeaponX() )
				return

			if ( IsValidItemFlavorGUID( weapon.GetItemFlavorGUID() ) )
			{
				ArtifactConfig artifactConfig
				artifactConfig.powerSource = GetItemFlavorByGUID( weapon.GetItemFlavorGUID() )

				
				EHI ownerEHI = ToEHI( player )
				ItemFlavor character = LoadoutSlot_GetItemFlavor( ownerEHI, Loadout_Character() )
				artifactConfig.blade = LoadoutSlot_GetItemFlavor( ownerEHI, Loadout_MeleeSkin( character ) )
				artifactConfig.character = character

				if ( ItemFlavor_GetType( artifactConfig.blade ) != eItemType.artifact_component_blade )
				{
					Warning( "Artifacts: received invalid ItemType for blade GUID." )
					artifactConfig.blade = fileLevel.componentSets[ BASE_SET_INDEX ][ eArtifactComponentType.BLADE ]
				} 

				player.p.artifactConfig = artifactConfig
			}

			return
		}


	if ( player.IsBot() )
		return

	EHI playerEHI = ToEHI( player )

	ItemFlavor character = LoadoutSlot_GetItemFlavor( playerEHI, Loadout_Character() )

	if ( !forceClear && player.p.artifactConfig != null )
	{
		ArtifactConfig artifactConfig = expect ArtifactConfig( player.p.artifactConfig )
		if ( artifactConfig.character == character ) 
			return
	}

	ItemFlavor meleeSkin = LoadoutSlot_GetItemFlavor( playerEHI, Loadout_MeleeSkin( character ) )

	Assert( IsLobby() || Artifacts_Loadouts_IsConfigPointerItemFlavor( meleeSkin ) ) 

	int configIdx = 0
	if ( Artifacts_Loadouts_IsConfigPointerItemFlavor( meleeSkin ) )
		configIdx = Artifacts_Loadouts_GetConfigIndex( meleeSkin )

	LoadoutEntry bladeEntry   = Artifacts_Loadouts_GetEntryForConfigIndexAndType( configIdx, eArtifactComponentType.BLADE )
	ItemFlavor bladeComponent = LoadoutSlot_GetItemFlavor( playerEHI, bladeEntry )
	bladeComponent = Artifacts_IsEmptyComponent( bladeComponent ) ? fileLevel.componentSets[ BASE_SET_INDEX ][ eArtifactComponentType.BLADE ] : bladeComponent

	LoadoutEntry powerSourceEntry   = Artifacts_Loadouts_GetEntryForConfigIndexAndType( configIdx, eArtifactComponentType.POWER_SOURCE )
	ItemFlavor powerSourceComponent = LoadoutSlot_GetItemFlavor( playerEHI, powerSourceEntry )

	LoadoutEntry deathboxEntry   = Artifacts_Loadouts_GetEntryForConfigIndexAndType( configIdx, eArtifactComponentType.DEATHBOX )
	ItemFlavor deathboxComponent = LoadoutSlot_GetItemFlavor( playerEHI, powerSourceEntry )

	LoadoutEntry themeEntry   = Artifacts_Loadouts_GetEntryForConfigIndexAndType( configIdx, eArtifactComponentType.THEME )
	ItemFlavor themeComponent = LoadoutSlot_GetItemFlavor( playerEHI, themeEntry )

	LoadoutEntry activationEmoteEntry   = Artifacts_Loadouts_GetEntryForConfigIndexAndType( configIdx, eArtifactComponentType.ACTIVATION_EMOTE )
	ItemFlavor activationEmoteComponent = LoadoutSlot_GetItemFlavor( playerEHI, powerSourceEntry )

	ArtifactConfig artifactConfig

	artifactConfig.character		   = character
	artifactConfig.blade               = bladeComponent
	artifactConfig.powerSource         = powerSourceComponent
	artifactConfig.deathbox            = deathboxComponent
	artifactConfig.theme               = themeComponent
	artifactConfig.activationEmote     = activationEmoteComponent

	player.p.artifactConfig = artifactConfig
}



void function Artifacts_OnWeaponOwnerChanged( entity weapon, entity owner )
{
	Assert( owner.IsBot() || owner.p.artifactConfig != null )
	if ( owner.p.artifactConfig == null )
		return


		if ( owner != GetLocalClientPlayer() )
			return


	

	EHI ownerEHI         = ToEHI( owner )
	int tier             = Artifacts_Loadouts_GetEquippedTier( ownerEHI )
	int componentChanged = 0

	string bladeSkinName
	string powerSourceModifierString

	ArtifactConfig artifactConfig = expect ArtifactConfig( owner.p.artifactConfig )

	bladeSkinName = Artifacts_GetSetKey( artifactConfig.blade )
	bladeSkinName = bladeSkinName.tolower()

	powerSourceModifierString = Artifacts_GetSetKey( artifactConfig.powerSource )
	powerSourceModifierString = powerSourceModifierString.tolower()
	powerSourceModifierString += ARTIFACT_POWER_SOURCE_MODIFIER_SUFFIX

	int GUID = Artifacts_GetComponentChangeGUID( owner )
	if ( GUID != 0 )
	{
		if ( GUID == ItemFlavor_GetGUID( artifactConfig.blade ) )
		{
			componentChanged = WEAPON_ARTIFACT_BLADE_CHANGED
		}
		else
		{
			if ( GUID == ItemFlavor_GetGUID( artifactConfig.powerSource ) )
				componentChanged = WEAPON_ARTIFACT_POWERSOURCE_CHANGED
		}
	}

	weapon.SetupArtifact( tier, bladeSkinName, powerSourceModifierString, componentChanged )
}



ArtifactViewmodelData ornull function ClientCodeCallback_GetArtifactViewmodelDataForWeapon( entity weapon )
{
	if ( !weapon.GetWeaponSettingBool( eWeaponVar.is_artifact ) )
		return null

	entity owner = weapon.GetOwner()
	if ( !IsValid( owner ) )
	{
		Warning( "Code called GetArtifactViewmodelDataForWeapon on a weapon that has no owner\n")
		return null
	}

	if ( owner.IsBot() )
		return null 

	int powerGUID = weapon.GetItemFlavorGUID()
	Assert( IsValidItemFlavorGUID( powerGUID ) )
	if ( !IsValidItemFlavorGUID( powerGUID ) )
		return null

	int bladeGUID = weapon.GetWeaponCharmOrArtifactBladeGUID()

	ArtifactViewmodelData res
	res.powerSourceGUID = powerGUID
	res.bladeGUID = IsValidItemFlavorGUID( bladeGUID) ? bladeGUID : ASSET_SETTINGS_UNIQUE_ID_INVALID

	return res
}



void function Artifacts_Loadouts_SetupWeaponComponents( entity weapon, entity owner )
{
#if DEV
		if ( weapon.IsWeaponX() )
			DEV_UpdatePlayerArtifactConfiguration( weapon, owner ) 
#endif

	Assert( owner.IsBot() || owner.p.artifactConfig != null )
	if ( owner.p.artifactConfig == null )
		return

	ArtifactConfig artifactConfig = expect ArtifactConfig( owner.p.artifactConfig )

	weapon.kv.rendercolor = Artifacts_FX_GetEmissiveAndSmearColor( artifactConfig.powerSource, true )























	if ( weapon.IsWeaponX() )
		weapon.SetWeaponCharmOrArtifactBladeGUID( artifactConfig.blade.guid )
}

void function Artifacts_Loadouts_ApplyModelForSet( entity weapon, int setIndex )
{






		Assert( weapon.IsClientOnly(), weapon + " isn't client only" )
		Assert( weapon.GetCodeClassName() == "dynamicprop", weapon + " has classname \"" + weapon.GetCodeClassName() + "\" instead of \"dynamicprop\"" )
		weapon.SetModel( fileLevel.setModels[ setIndex ].viewModel ) 

}

bool function _SetComponentModByIdx( entity weapon, int idx, string bodyGroupName )
{
	if ( weapon.HasMod( bodyGroupName + idx ) )
		return false

	
	int count = ( bodyGroupName == POWER_SOURCE_BODY_GROUP_MOD_PREFIX ) ? eArtifactSetIndex.COUNT + 1 : eArtifactSetIndex.COUNT
	for ( int i = 0; i < count; i++ ) 
	{
		if ( i == idx )
			continue

		weapon.RemoveMod( bodyGroupName + idx )
	}
	weapon.AddMod( bodyGroupName + idx )
	return true
}



void function _PreviewComponent( entity weapon, int previewIdx, string bodyGroupName )
{
	Assert( IsLobby() )

	int bodygroupIdx = weapon.FindBodygroup( bodyGroupName )
	if ( bodygroupIdx != BODY_GROUP_INVALID )
		weapon.SetBodygroupModelByIndex( bodygroupIdx, previewIdx )
}

void function Artifacts_ClearMenuEffects()
{
	foreach ( int fxHandle in fileLevel.lobbyFxHandles )
	{
		if ( EffectDoesExist( fxHandle ) )
			EffectStop( fxHandle, true, false )
	}

	fileLevel.lobbyFxHandles.clear()
}

void function Artifacts_Loadouts_PreviewBladeAndPowerSource( entity weapon, ItemFlavor blade, ItemFlavor powerSource )
{
	Assert( IsLobby() )

	if ( Artifacts_IsEmptyComponent( blade ) )
		return

	_PreviewComponent( weapon, eArtifactSetIndex[ Artifacts_GetSetKey( blade ) ], BLADE_BODY_GROUP_MOD_PREFIX )

	
	int componentIdx = Artifacts_IsEmptyComponent( powerSource ) ? eArtifactSetIndex.COUNT : eArtifactSetIndex[ Artifacts_GetSetKey( powerSource ) ]
	_PreviewComponent( weapon, componentIdx, POWER_SOURCE_BODY_GROUP_MOD_PREFIX )

	Artifacts_ClearMenuEffects()

	Artifacts_FX_StartLobbyFX( weapon, powerSource, blade )
}

void function Artifacts_Loadouts_PreviewTheme( entity weapon, ItemFlavor theme )
{
	Assert( IsLobby() )

	Artifacts_Loadouts_SetBaseSkinForTheme( weapon, theme )
}


void function Artifacts_Loadouts_SetBaseSkinForTheme( entity weapon, ItemFlavor theme )
{
	if ( Artifacts_IsEmptyComponent( theme ) )
		weapon.SetSkin( THEME_BASE )
	else
		weapon.SetSkin( THEME_SHINY )
}

void function _SetBodyGroupMod( entity player, entity weapon, ItemFlavor component, string componentBodyGroup )
{
	Assert( IsValid( player ) && player.IsPlayer() )

	if ( Artifacts_IsEmptyComponent( component ) && Artifacts_GetComponentType( component ) != eArtifactComponentType.POWER_SOURCE )
		return

	int componentIdx = Artifacts_IsEmptyComponent( component ) ? eArtifactSetIndex.COUNT : eArtifactSetIndex[ Artifacts_GetSetKey( component ) ]
	if ( weapon.IsWeaponX() )
	{
		entity artifactWeap = weapon

		if ( IsValid( artifactWeap ) && artifactWeap.GetWeaponClassName() == ARTIFACT_DAGGER_MP_WEAPON )
		{
			entity artifactVM = artifactWeap.GetWeaponViewmodel()
			if ( artifactWeap.FindBodygroup( componentBodyGroup ) != BODY_GROUP_INVALID )
			{
				if ( _SetComponentModByIdx( artifactWeap, componentIdx, componentBodyGroup ) && IsValid( artifactVM ) )
					artifactVM.SetBodygroupModelByIndex( artifactWeap.FindBodygroup( componentBodyGroup ), componentIdx )
			}
		}

		entity meleeWeap = player.GetOffhandWeapon( WEAPON_INVENTORY_SLOT_ANTI_TITAN )
		if ( IsValid( meleeWeap ) && meleeWeap.GetWeaponClassName() == ARTIFACT_DAGGER_MELEE_WEAPON )
		{
			if ( meleeWeap.FindBodygroup( componentBodyGroup ) != BODY_GROUP_INVALID )
				_SetComponentModByIdx( meleeWeap, componentIdx, componentBodyGroup )
		}
	}
	else
	{
		if ( weapon.FindBodygroup( componentBodyGroup ) != BODY_GROUP_INVALID )
				weapon.SetBodygroupModelByIndex( weapon.FindBodygroup( componentBodyGroup ), componentIdx )
	}
}

void function Artifacts_Loadouts_SetBladeMod( entity player, entity weapon, ItemFlavor blade )
{
	_SetBodyGroupMod( player, weapon, blade, BLADE_BODY_GROUP_MOD_PREFIX )
}

void function Artifacts_Loadouts_SetPowerSourceMod( entity player, entity weapon, ItemFlavor powerSource )
{
	_SetBodyGroupMod( player, weapon, powerSource, POWER_SOURCE_BODY_GROUP_MOD_PREFIX )
}


int function Artifacts_Loadouts_GetConfigIndexForLoadoutSlot( LoadoutEntry slot )
{
	Assert ( slot in fileLevel.loadoutConfigurationSlotsToIndices )
	return fileLevel.loadoutConfigurationSlotsToIndices[ slot ]
}




































LoadoutEntry function Artifacts_Loadouts_GetEntryForConfigIndexAndType( int configIdx, int componentType )
{
	return fileLevel.loadoutConfigurationEntriesByIndexAndType[ configIdx ][ componentType ]
}

int function Artifacts_Loadouts_GetEquippedTier( EHI playerEHI )
{
	
	
	
	

#if DEV
		return GetConVarInt( "artifacts_tier_override" )
#endif

	return 0 
}



ItemFlavor function Artifacts_GetAssociatedWeaponForComponent( ItemFlavor component )
{
	
	return GetItemFlavorByAsset( GetGlobalSettingsAsset( ItemFlavor_GetAsset( component ), "parentItemFlavor" ) )
}




void function Artifacts_FX_StartLobbyFX( entity weapon, ItemFlavor powerSource, ItemFlavor blade )
{
	int bladeFx = ARTIFACT_FX_HANDLE_INVALID
	int bladeAttachIdx = -1
	foreach ( fxPackage in Artifacts_FX_GetLobbyFX( powerSource ) )
	{
		int fxIndex   = GetParticleSystemIndex( fxPackage.fxAsset )
		int attachIdx = weapon.LookupAttachment( fxPackage.attachName )
		int fxHandle  = StartParticleEffectOnEntity( weapon, fxIndex, FX_PATTACH_POINT_FOLLOW, attachIdx )

		fileLevel.lobbyFxHandles.append( fxHandle )

		if ( bladeFx == ARTIFACT_FX_HANDLE_INVALID && fxPackage.fxAsset.find( BLADE_BODY_GROUP_MOD_PREFIX ) != -1 )
		{
			bladeFx = fxHandle
			bladeAttachIdx = attachIdx
		}
	}

	if ( bladeFx != ARTIFACT_FX_HANDLE_INVALID )
	{
		array< ArtifactFXControlPoint > controlPoints = Artifacts_FX_GetBladeControlPoints( blade, true )
		foreach ( ArtifactFXControlPoint cp in controlPoints )
			EffectAddTrackingForControlPoint( bladeFx, cp.controlPointNumber, weapon, FX_PATTACH_POINT_FOLLOW, bladeAttachIdx, cp.controlPoint )
	}

	weapon.kv.rendercolor = Artifacts_FX_GetEmissiveAndSmearColor( powerSource, true )
}



ArtifactFX1PAnd3P function Artifacts_FX_Get1PAnd3PFXArraysFromPowerSource( ItemFlavor powerSource, int fxType )
{
	ArtifactFX1PAnd3P fx1P3P

	if ( Artifacts_IsEmptyComponent( powerSource ) )
		return fx1P3P

	array< ArtifactFXPackage > fxPackages
	switch ( fxType )
	{
		case eArtifactFXPackageType.ATTACK:
			fxPackages = Artifacts_FX_GetAttackFX( powerSource )
			break

		case eArtifactFXPackageType.IDLE:
			fxPackages = Artifacts_FX_GetIdleFX( powerSource )
			break

		case eArtifactFXPackageType.STARTUP:
			fxPackages = Artifacts_FX_GetStartupFX( powerSource )
			break

		case eArtifactFXPackageType.FLOURISH:
			fxPackages = Artifacts_FX_GetFlourishFX( powerSource )
			break

		case eArtifactFXPackageType.INSPECT:
			fxPackages = Artifacts_FX_GetInspectFX( powerSource )
			break

		case eArtifactFXPackageType.LOBBY:
			fxPackages = Artifacts_FX_GetLobbyFX( powerSource )
			break

		default:
			Assert( false )
	}

	
	foreach ( fxPackageStruct in fxPackages )
	{
		if ( fxPackageStruct.is1P )
			fx1P3P.fx1P.append( fxPackageStruct )
		else
			fx1P3P.fx3P.append( fxPackageStruct )
	}

	return fx1P3P
}

ArtifactFX1PAnd3P function Artifacts_FX_Get1PAnd3PFXArraysForWeapon( entity weapon, int fxType )
{
	if ( !IsValidItemFlavorGUID( weapon.GetItemFlavorGUID() ) )
	{
		ArtifactFX1PAnd3P fx1P3P
		return fx1P3P
	}

	return Artifacts_FX_Get1PAnd3PFXArraysFromPowerSource( GetItemFlavorByGUID( weapon.GetItemFlavorGUID() ), fxType )
}

void function Artifact_PrecacheImpactTables( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )

	if ( IsLobby() )
		return

	foreach ( string impactTable in GetSkinImpactTableAliases( ItemFlavor_GetGUID( powerSource ) ) )
		PrecacheImpactEffectTable( impactTable )
}

void function Artifact_PrecachePowerSourceFX( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )

	if ( !IsLobby() )
	{
		foreach ( ArtifactFXPackage fxPackageStruct in Artifacts_FX_GetIdleFX( powerSource ) )
			PrecacheParticleSystem( fxPackageStruct.fxAsset )

		foreach ( ArtifactFXPackage fxPackageStruct in Artifacts_FX_GetFlourishFX( powerSource ) )
			PrecacheParticleSystem( fxPackageStruct.fxAsset )

		foreach ( ArtifactFXPackage fxPackageStruct in Artifacts_FX_GetAttackFX( powerSource ) )
			PrecacheParticleSystem( fxPackageStruct.fxAsset )

		foreach ( ArtifactFXPackage fxPackageStruct in Artifacts_FX_GetStartupFX( powerSource ) )
			PrecacheParticleSystem( fxPackageStruct.fxAsset )

		foreach ( ArtifactFXPackage fxPackageStruct in Artifacts_FX_GetInspectFX( powerSource ) )
			PrecacheParticleSystem( fxPackageStruct.fxAsset )
	}
	else
	{
		foreach ( ArtifactFXPackage fxPackageStruct in Artifacts_FX_GetLobbyFX( powerSource ) )
			PrecacheParticleSystem( fxPackageStruct.fxAsset )
	}
}

void function Artifact_PrecacheDeathboxModelAndFX( ItemFlavor deathbox )
{
	Assert( ItemFlavor_GetType( deathbox ) == eItemType.artifact_component_deathbox )
	asset deathboxMdl = Artifacts_GetDeathboxModel( deathbox )
	if ( deathboxMdl != $"" )
	{
#if DEV
			printf( "ArtifactsDebug: precaching Deathbox model %s", string(deathboxMdl) )
#endif
		PrecacheModel( deathboxMdl )
	}

	asset particleFX = Artifacts_FX_GetDeathboxFX( deathbox )
	if ( particleFX != $"" )
	{
#if DEV
			printf( "ArtifactsDebug: precaching Deathbox FX %s", string(particleFX) )
#endif
		PrecacheParticleSystem( particleFX )
	}
}

array<ArtifactFXPackage> function Artifacts_FX_GetPackage( ItemFlavor powerSource, string packageName, int packageType )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )
	Assert( FX_PACKAGES.contains( packageName ) )
	Assert( packageType > eArtifactFXPackageType.INVALID && packageType < eArtifactFXPackageType.COUNT )

	array<ArtifactFXPackage> fxPackageStructs = []
	foreach ( var fxPackage in IterateSettingsAssetArray( ItemFlavor_GetAsset( powerSource ), packageName ) )
	{
		ArtifactFXPackage fxPackageStruct
		fxPackageStruct.fxPackageType = packageType
		fxPackageStruct.fxAsset = GetSettingsBlockStringAsAsset( fxPackage, FX_ASSET )
		fxPackageStruct.attachName = GetSettingsBlockString( fxPackage, FX_ATTACH )
		fxPackageStruct.is1P = ( GetSettingsBlockString( fxPackage, FX_PERSPECTIVE ) == ONE_P ) ? true : false

		fxPackageStructs.append( fxPackageStruct )
	}

	return fxPackageStructs
}

array<ArtifactFXPackage> function Artifacts_FX_GetIdleFX( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )
	return Artifacts_FX_GetPackage( powerSource, FX_PACKAGE_IDLE, eArtifactFXPackageType.IDLE )
}

array<ArtifactFXPackage> function Artifacts_FX_GetFlourishFX( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )
	return Artifacts_FX_GetPackage( powerSource, FX_PACKAGE_FLOURISH, eArtifactFXPackageType.FLOURISH )
}

array<ArtifactFXPackage> function Artifacts_FX_GetAttackFX( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )
	return Artifacts_FX_GetPackage( powerSource, FX_PACKAGE_ATTACK, eArtifactFXPackageType.ATTACK )
}

array<ArtifactFXPackage> function Artifacts_FX_GetStartupFX( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )
	return Artifacts_FX_GetPackage( powerSource, FX_PACKAGE_STARTUP, eArtifactFXPackageType.STARTUP )
}

array<ArtifactFXPackage> function Artifacts_FX_GetInspectFX( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )
	return Artifacts_FX_GetPackage( powerSource, FX_PACKAGE_INSPECT, eArtifactFXPackageType.INSPECT )
}

array<ArtifactFXPackage> function Artifacts_FX_GetLobbyFX( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )
	return Artifacts_FX_GetPackage( powerSource, FX_PACKAGE_LOBBY, eArtifactFXPackageType.LOBBY )
}

vector function Artifacts_FX_GetEmissiveAndSmearColor( ItemFlavor powerSource, bool checkEmptyForEmissive )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )

	if ( checkEmptyForEmissive && Artifacts_IsEmptyComponent( powerSource ) )
		return FX_NULL_CAP_EMISSIVE

	var settingsBlock = ItemFlavor_GetSettingsBlock( powerSource )
	return ( GetSettingsBlockVector( settingsBlock, FX_SMEAR_COLOR ) * 255 )
}

int function Artifacts_FX_GetSkinIndex( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )
	var settingsBlock = ItemFlavor_GetSettingsBlock( powerSource )
	return GetSettingsBlockInt( settingsBlock, FX_SKIN_INDEX )
}

asset function Artifacts_FX_GetDeathboxFX( ItemFlavor flav )
{
	Assert( ItemFlavor_GetType( flav ) == eItemType.artifact_component_deathbox )
	Assert( Artifacts_GetComponentType( flav ) == eArtifactComponentType.DEATHBOX )

	var block = ItemFlavor_GetSettingsBlock( flav )
	return GetSettingsBlockStringAsAsset( block, DEATHBOX_FX_NAME_KEY )
}

string function Artifacts_FX_GetDeathboxSpawnSFX( ItemFlavor flav )
{
	Assert( ItemFlavor_GetType( flav ) == eItemType.artifact_component_deathbox )
	Assert( Artifacts_GetComponentType( flav ) == eArtifactComponentType.DEATHBOX )

	var block = ItemFlavor_GetSettingsBlock( flav )
	return GetSettingsBlockString( block, DEATHBOX_SFX_SPAWN_KEY )
}

asset function Artifacts_GetDeathboxModel( ItemFlavor flav )
{
	Assert( ItemFlavor_GetType( flav ) == eItemType.artifact_component_deathbox )
	Assert( Artifacts_GetComponentType( flav ) == eArtifactComponentType.DEATHBOX )

	var block = ItemFlavor_GetSettingsBlock( flav )
	return GetSettingsBlockAsset( block, DEATHBOX_MDL_REF_KEY )
}

array< ArtifactFXControlPoint > function Artifacts_FX_GetBladeControlPoints( ItemFlavor flav, bool forLobby )
{
	Assert( ItemFlavor_GetType( flav ) == eItemType.artifact_component_blade )
	Assert( Artifacts_GetComponentType( flav ) == eArtifactComponentType.BLADE )

	array< ArtifactFXControlPoint > bladeControlPoints = []
	var block = ItemFlavor_GetSettingsBlock( flav )

	string settingsKey = FX_CONTROL_POINT_LIST
	if ( forLobby )
		settingsKey = FX_CONTROL_POINT_LIST_LOBBY

	foreach ( var fxControlPointBlock in IterateSettingsArray( GetSettingsBlockArray( block, settingsKey ) ) )
	{
		ArtifactFXControlPoint fxCP
		fxCP.controlPoint       = GetSettingsBlockVector( fxControlPointBlock, FX_CONTROL_POINT )
		fxCP.controlPointNumber = GetSettingsBlockInt( fxControlPointBlock, FX_CONTROL_POINT_NUMBER )
		fxCP.controlPointName   = GetSettingsBlockString( fxControlPointBlock, FX_CONTROL_POINT_NAME )
		bladeControlPoints.append( fxCP )
	}

	return bladeControlPoints
}


table< int, array< FXHandle > > s_fxHandles


void function Artifacts_FX_StartWeaponFX( entity weapon, int fxPackageType )
{
	int weaponEffect		 = ARTIFACT_FX_HANDLE_INVALID
	entity effectEntity		 = null
	bool isWeaponEnt		 = weapon.IsWeaponX()

	
	if ( fxPackageType == eArtifactFXPackageType.BLADE_EMISSIVE )
	{
		if ( !IsValidItemFlavorGUID( weapon.GetItemFlavorGUID() ) )
			return

		thread function() : ( weapon ) {
			weapon.EndSignal( ARTIFACT_EMISSIVE_FX_SIGNAL )
			weapon.EndSignal( "WeaponDeactivateEvent" )

			vector finalColor = Artifacts_FX_GetEmissiveAndSmearColor( GetItemFlavorByGUID( weapon.GetItemFlavorGUID() ), true )
			float emissiveFactor = DEACTIVATED_EMISSIVE_FACTOR
			weapon.kv.rendercolor = finalColor * DEACTIVATED_EMISSIVE_FACTOR
			while ( emissiveFactor <= ( 1.0 - FLT_EPSILON ) && IsValid( weapon ) )
			{
				emissiveFactor = emissiveFactor + 0.05
				weapon.kv.rendercolor = finalColor * emissiveFactor
				WaitFrame()
			}
			if ( IsValid( weapon ) )
				weapon.kv.rendercolor = finalColor
		}()

		return
	}

	ArtifactFX1PAnd3P fx1P3P = Artifacts_FX_Get1PAnd3PFXArraysForWeapon( weapon, fxPackageType )
	int fxLen = maxint( fx1P3P.fx1P.len(), fx1P3P.fx3P.len() )
	for ( int i = 0; i < fxLen; i++ )
	{
		asset onePAsset   = fx1P3P.fx1P.isvalidindex( i ) ? fx1P3P.fx1P[ i ].fxAsset : $""
		string attachName = fx1P3P.fx1P.isvalidindex( i ) ? fx1P3P.fx1P[ i ].attachName : fx1P3P.fx3P[ i ].attachName
		Assert( attachName != "" )
		asset threePAsset = fx1P3P.fx3P.isvalidindex( i ) ? fx1P3P.fx3P[ i ].fxAsset : $""


			entity player = weapon.GetOwner()
			bool is3PActive = player.IsThirdPersonShoulderModeOn() || GetConVarInt( "thirdperson_override" ) == 1
			bool isLocal1PFlourish = ( fxPackageType == eArtifactFXPackageType.FLOURISH && player == GetLocalViewPlayer() && !is3PActive )
			if ( isLocal1PFlourish )
			{
				entity weaponVm = weapon.GetWeaponViewmodel()
				int fxHandle = StartParticleEffectOnEntity( weaponVm, GetParticleSystemIndex( onePAsset ), FX_PATTACH_POINT_FOLLOW, weaponVm.LookupAttachment( attachName ) )
				if ( !( fxPackageType in s_fxHandles ) )
					s_fxHandles[ fxPackageType ] <- []
				s_fxHandles[ fxPackageType ].append( fxHandle )
				return
			}












			weapon.PlayWeaponEffect( onePAsset, threePAsset, attachName, true )
	}
}

void function Artifacts_FX_StopWeaponFX( entity weapon, int fxPackageType )
{
	
	if ( fxPackageType == eArtifactFXPackageType.BLADE_EMISSIVE )
	{
		if ( !IsValidItemFlavorGUID( weapon.GetItemFlavorGUID() ) )
			return

		weapon.Signal( ARTIFACT_EMISSIVE_FX_SIGNAL )
		weapon.kv.rendercolor = DEACTIVATED_EMISSIVE_FACTOR * Artifacts_FX_GetEmissiveAndSmearColor( GetItemFlavorByGUID( weapon.GetItemFlavorGUID() ), true )
		return
	}


		entity player = weapon.GetOwner()
		bool is3PActive = player.IsThirdPersonShoulderModeOn() || GetConVarInt( "thirdperson_override" ) == 1
		bool isLocal1PFlourish = ( fxPackageType == eArtifactFXPackageType.FLOURISH && player == GetLocalViewPlayer() && !is3PActive )
		if ( isLocal1PFlourish && fxPackageType in s_fxHandles )
		{
			array< FXHandle > allFxHandles = clone ( s_fxHandles[ fxPackageType ] )
			foreach ( FXHandle handle in allFxHandles )
			{
				if ( EffectDoesExist( handle ) )
					EffectStop( handle, false, true )
			}
			delete s_fxHandles[ fxPackageType ]
			Remote_ServerCallFunction( "ClientCallback_ActivationEmote", false )
			return
		}








	ArtifactFX1PAnd3P fx1P3P = Artifacts_FX_Get1PAnd3PFXArraysForWeapon( weapon, fxPackageType )
	int fxLen = maxint( fx1P3P.fx1P.len(), fx1P3P.fx3P.len() )
	for ( int i = 0; i < fxLen; i++ )
	{
		asset onePAsset   = ( fx1P3P.fx1P.isvalidindex( i ) && fxPackageType != eArtifactFXPackageType.FLOURISH ) ? fx1P3P.fx1P[ i ].fxAsset : $""
		asset threePAsset = fx1P3P.fx3P.isvalidindex( i ) ? fx1P3P.fx3P[ i ].fxAsset : $""
		weapon.StopWeaponEffect( onePAsset, threePAsset )
	}
}

void function Artifacts_FX_ScriptAnimWindowCallback( entity weapon, string parameter, bool isWindowStart )
{
	if ( !IsValidItemFlavorGUID( weapon.GetItemFlavorGUID() ) )
		return

	ItemFlavor powerSource = GetItemFlavorByGUID( weapon.GetItemFlavorGUID() )

	if ( Artifacts_IsEmptyComponent( powerSource ) )
		return

	int setIndex = Artifacts_GetSetIndex( powerSource )

	ArtifactFX1PAnd3P fx1P3P
	var scriptAnimBlock = ItemFlavor_GetSettingsBlock( GetItemFlavorByGUID( ARTIFACT_DAGGER_ITEM_FLAVOR_GUID ) )
	foreach ( var paramsBlock in IterateSettingsArray( GetSettingsBlockArray( scriptAnimBlock, SCRIPT_ANIM_WINDOW_FX ) ) )
	{
		if ( parameter == GetSettingsBlockString( paramsBlock, SCRIPT_ANIM_WINDOW_PARAMETER ) )
		{
			foreach ( var fxBlock in IterateSettingsArray( GetSettingsBlockArray( paramsBlock, SCRIPT_ANIM_WINDOW_SET_FX ) ) )
			{
				if ( SCRIPT_ANIM_WINDOW_UNIVERSAL == GetSettingsBlockString( fxBlock, THEME_NAME ) || setIndex == eArtifactSetIndex[ GetSettingsBlockString( fxBlock, THEME_NAME ) ] )
				{
					int fxPackageType = eArtifactFXPackageType[ GetSettingsBlockString( fxBlock , SCRIPT_ANIM_WINDOW_FX_PACKAGE_TYPE ) ]
					bool shouldStart  = ( isWindowStart && GetSettingsBlockString( fxBlock , SCRIPT_ANIM_WINDOW_ACTION ) == SCRIPT_ANIM_WINDOW_START ) ||
											( !isWindowStart && GetSettingsBlockString( fxBlock , SCRIPT_ANIM_WINDOW_ACTION ) == SCRIPT_ANIM_WINDOW_STOP )

					if ( shouldStart )
						Artifacts_FX_StartWeaponFX( weapon, fxPackageType )
					else
						Artifacts_FX_StopWeaponFX( weapon, fxPackageType )

					break
				}
			}

			break
		}
	}
}

bool function Artifacts_PlayerHasArtifactActive( entity player )
{
	entity activeWeapon = player.GetActiveWeapon( eActiveInventorySlot.mainHand )
	return ( IsValid( activeWeapon ) && activeWeapon.IsWeaponX() && activeWeapon.GetWeaponSettingBool( eWeaponVar.is_artifact ) )
}





































































































int function Artifacts_GetComponentChangeGUID( entity player )
{

	Assert( player == GetLocalClientPlayer() )


	
	ItemFlavor changedComponent = LoadoutSlot_GetItemFlavor( ToEHI( player ), fileLevel.componentChangeSlot )
	if ( Artifacts_IsEmptyComponent( changedComponent ) )
		return 0

	return ItemFlavor_GetGUID( changedComponent )
}























































































































bool function Artifacts_IsBaseArtifactOwned( entity player = null )
{
	array< ItemFlavor > baseSet = fileLevel.componentSets[ BASE_SET_INDEX ]

	if ( baseSet.len() == 0 )
		return false

	ItemFlavor baseArtifact = baseSet[0]








		return GRX_IsItemOwnedByPlayer( baseArtifact )


	unreachable
}


array< ItemFlavor > function Artifacts_GetSetItems( int setIndex )
{
	return fileLevel.componentSets[ setIndex ]
}

bool function Artifacts_IsBaseArtifact( ItemFlavor component )
{
	return ItemFlavor_GetType( component ) == eItemType.artifact_component_blade && Artifacts_GetSetIndex( component ) == BASE_SET_INDEX
}

asset function Artifacts_ActivationEmote_GetVideo( ItemFlavor emote )
{
	Assert( ItemFlavor_GetType( emote ) == eItemType.artifact_component_activation_emote )

	return GetGlobalSettingsStringAsAsset( ItemFlavor_GetAsset( emote ), "video" )
}


#if DEV
























void function DEV_UpdatePlayerArtifactConfiguration( entity weapon, entity player )
{
	if ( player != GetLocalClientPlayer() )
		return

	if ( player.p.artifactConfig != null )
	{
		ArtifactConfig artifactConfig = expect ArtifactConfig( player.p.artifactConfig )
		if ( weapon.HasMod( POWER_SOURCE_BODY_GROUP_MOD_PREFIX + Artifacts_GetSetIndex( artifactConfig.powerSource ) ) )
			return
	}

	Artifacts_StoreLoadoutDataOnPlayerEntityStruct( player, null, true )
}

































































































































































































































































void function Artifacts_DEV_UnitTest_PowerSource( ItemFlavor powerSource )
{
	Artifacts_FX_GetIdleFX( powerSource )
	Artifacts_FX_GetFlourishFX( powerSource )
	Artifacts_FX_GetAttackFX( powerSource )
	Artifacts_FX_GetStartupFX( powerSource )
	Artifacts_FX_GetEmissiveAndSmearColor( powerSource, true )
	Artifacts_FX_GetEmissiveAndSmearColor( powerSource, false )
	Artifacts_FX_GetSkinIndex( powerSource )
}

void function Artifacts_DEV_UnitTest_Deathbox( ItemFlavor deathbox )
{
	Artifacts_FX_GetDeathboxFX( deathbox )
	Artifacts_GetDeathboxModel( deathbox )
}

void function Artifacts_DEV_UnitTest_Blade( ItemFlavor blade )
{
	Artifacts_FX_GetBladeControlPoints( blade, true )
	Artifacts_FX_GetBladeControlPoints( blade, false )
}

#endif
