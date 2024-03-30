global function ShArtifacts_LevelInit

global function RegisterArtifactComponentsForWeapon
global function Artifacts_GetConfigurationFramework
global function Artifacts_GetAssociatedWeaponForComponent
global function Artifacts_GetComponentType
global function Artifacts_GetSetKey
global function Artifacts_GetSetIndex
global function Artifacts_GetSetNameLocalized
global function Artifacts_GetComponentChangeGUID
global function Artifacts_IsEmptyComponent


global function Artifacts_StoreLoadoutDataOnPlayerEntityStruct
global function Artifacts_OnWeaponOwnerChanged
global function Artifact_PrecacheDeathboxModelAndFX



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


global function Artifacts_Loadouts_PreviewBladeAndPowerSource
global function Artifacts_Loadouts_PreviewTheme











global function Artifact_Set1pFxControlPoints
global function ServerCallback_Artifacts_SetPlayerArtifactViewmodelData
global function ClientCodeCallback_GetArtifactViewmodelDataForWeapon










global function Artifacts_FX_StartWeaponFX
global function Artifacts_FX_StopWeaponFX
global function Artifacts_FX_ScriptAnimWindowCallback

global function Artifacts_FX_GetIdleFX
global function Artifacts_FX_GetFlourishFX
global function Artifacts_FX_GetAttackFX
global function Artifacts_FX_GetStartupFX
global function Artifacts_FX_GetLobbyFX
global function Artifacts_FX_GetSmearColor
global function Artifacts_FX_GetSkinIndex
global function Artifacts_FX_GetImpactTable
global function Artifacts_FX_Get1PAnd3PFXArraysFromPlayerLoadouts

global function Artifacts_FX_GetDeathboxFX
global function Artifacts_GetDeathboxModel
global function Artifacts_FX_GetDeathboxSpawnSFX

global function Artifacts_FX_GetBladeControlPoints

global function Artifacts_PlayActivationEmote
global function Artifacts_PlayerHasArtifactActive


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

const string WORLD_MODEL = "worldModel"
const string VIEW_MODEL = "viewModel"

const string BASE_WEAPON = "base_weapon"
const table< int, string > ARTIFACT_COMPONENTS_TO_LOADOUT_NAMES_MAP = {
	[eArtifactComponentType.BLADE] = "blade",
	[eArtifactComponentType.THEME] = "theme",
	[eArtifactComponentType.POWER_SOURCE] = "power_source",
	[eArtifactComponentType.DEATHBOX] = "deathbox",
	[eArtifactComponentType.ACTIVATION_EMOTE] = "activation_emote",
}


const string MOB = "MOB"
const string EMPTY = "_EMPTY"
const table< string, string > THEME_NAMES_TO_LOC_KEYS = {
	[EMPTY] = "#ARTIFACT_THEME_EMPTY",
	[MOB] = "#ARTIFACT_THEME_MOBSTER",







}

global enum eArtifactSetIndex { 
	_EMPTY = -1, 





	MOB = 3,




	COUNT = 6 
}

const array<string> ARTIFACT_COMPONENT_SETTINGS_KEYS = [
	"blade",
	"theme",
	"powerSource",
	"deathbox",
	"activationEmote",
]


const int ARTIFACT_MAX_LOADOUTS = 1



const string ONE_P = "1P"
const string THREE_P = "3P"
const int BODY_GROUP_INVALID = -1

const int THEME_BASE = 1 
const int THEME_SHINY = 2 


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

const string FX_IMPACT_TABLE = "impactFXTable"
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



const asset VFX_TEST_DEATHBOX_PARTICLE = $"P_death_box_mob_kill_fx"
const asset VFX_TEST_IDLE = $"P_car_reac_spinners_lvl4"

const asset VFX_MOB_STARTUP_1P = $"P_art_MOB_power_start_FP"
const asset VFX_MOB_STARTUP_3P = $"P_art_MOB_power_start_3P"
const asset VFX_MOB_IDLE_1P = $"P_art_MOB_power_idle_FP"
const asset VFX_MOB_IDLE_3P = $"P_art_MOB_power_idle_3P"
const asset VFX_MOB_IDLE_BLADE_1P = $"P_art_MOB_blade_idle_FP"
const asset VFX_MOB_IDLE_BLADE_3P = $"P_art_MOB_blade_idle_3P"
const asset VFX_MOB_ATTACK_1P = $"P_art_MOB_blade_attack_FP"
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
const asset VFX_celes_ATTACK_1P = $"P_art_celes_blade_attack_FP"
const asset VFX_celes_ATTACK_3P = $"P_art_celes_blade_attack_3P"
const asset VFX_celes_FLOURISH_1P = $"P_art_celes_blade_flourish_FP"
const asset VFX_celes_FLOURISH_3P = $"P_art_celes_blade_flourish_3P"
const asset VFX_celes_INSPECT_1P = $"P_art_celes_power_inspect_FP"

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

	


	
	Remote_RegisterServerFunction( "ClientCallback_ActivationEmote" )
	Remote_RegisterClientFunction( "ServerCallback_Artifacts_SetPlayerArtifactViewmodelData", "entity", "int", 0, INT_MAX, "int", 0, INT_MAX )





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
		if ( setTheme != EMPTY )
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


		foreach ( string componentKey in ARTIFACT_COMPONENT_SETTINGS_KEYS )
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
					componentsInSet[ Artifacts_GetComponentType( component ) ] = component


					if ( Artifacts_GetComponentType( component ) == eArtifactComponentType.POWER_SOURCE )
						Artifact_PrecachePowerSourceFX( component )
					else if ( !IsLobby() && Artifacts_GetComponentType( component ) == eArtifactComponentType.DEATHBOX )
						Artifact_PrecacheDeathboxModelAndFX( component )


					string themeName = Artifacts_GetSetKey( component )
					Assert( themeName in THEME_NAMES_TO_LOC_KEYS )
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
		}

		fileLevel.componentSets[ eArtifactSetIndex[ currentTheme ] ] <- componentsInSet
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

			
			if ( componentCounter == eArtifactComponentType.DEATHBOX )
			{
				ItemFlavor ornull ghDeathbox = Deathbox_GetGoldenHorseDeathbox()
				if ( ghDeathbox != null && !fileLevel.componentListsByType[ eArtifactComponentType.DEATHBOX ].contains( expect ItemFlavor( ghDeathbox ) ) )
					fileLevel.componentListsByType[ eArtifactComponentType.DEATHBOX ].append( expect ItemFlavor( ghDeathbox ) )
			}

			componentEntry.defaultItemFlavor   = fileLevel.componentSets[ eArtifactSetIndex._EMPTY ][ componentCounter ] 

			if ( componentCounter == eArtifactComponentType.ACTIVATION_EMOTE ) 
			{
				componentEntry.validItemFlavorList.clear()
				componentEntry.validItemFlavorList.append( fileLevel.componentSets[ eArtifactSetIndex._EMPTY ][ componentCounter ] )
			}
			else

			{
				componentEntry.validItemFlavorList = fileLevel.componentListsByType[ componentCounter ]
			}

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
}

bool function Artifacts_IsEmptyComponent( ItemFlavor component )
{
	Assert( ItemFlavor_GetType( component ) > eItemType.artifact_component_START && ItemFlavor_GetType( component ) < eItemType.artifact_component_END )
	return ( GetGlobalSettingsBool( ItemFlavor_GetAsset( component ), IS_EMPTY ) )
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

string function Artifacts_GetSetNameLocalized( ItemFlavor component )
{
	Assert( ItemFlavor_GetType( component ) > eItemType.artifact_component_START && ItemFlavor_GetType( component ) < eItemType.artifact_component_END )
	return THEME_NAMES_TO_LOC_KEYS[ GetSettingsBlockString( ItemFlavor_GetSettingsBlock( component ), THEME_NAME ) ]
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
	if ( Artifacts_Loadouts_IsConfigPointerItemFlavor( meleeSkin ) )
	{
		LoadoutEntry bladeSlot    = Artifacts_Loadouts_GetEntryForConfigIndexAndType( Artifacts_Loadouts_GetConfigIndex( meleeSkin ), eArtifactComponentType.BLADE )
		ItemFlavor bladeComponent = LoadoutSlot_GetItemFlavor( playerEHI, bladeSlot )
#if DEV
			isMisconfigured = Artifacts_IsEmptyComponent( bladeComponent )
#else
			isMisconfigured = Artifacts_IsEmptyComponent( bladeComponent ) || !IsItemFlavorUnlockedForLoadoutSlot( playerEHI, bladeSlot, bladeComponent )
#endif

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


void function Artifacts_StoreLoadoutDataOnPlayerEntityStruct( entity player )
{
#if !DEV
		if ( player.p.artifactConfig != null ) 
			return
#endif

	if ( player.IsBot() )
		return

	EHI playerEHI = ToEHI( player )

	ItemFlavor character = LoadoutSlot_GetItemFlavor( playerEHI, Loadout_Character() )
	ItemFlavor meleeSkin = LoadoutSlot_GetItemFlavor( playerEHI, Loadout_MeleeSkin( character ) )

	Assert( Artifacts_Loadouts_IsConfigPointerItemFlavor( meleeSkin ) )

	int configIdx = Artifacts_Loadouts_GetConfigIndex( meleeSkin )

	LoadoutEntry bladeEntry   = Artifacts_Loadouts_GetEntryForConfigIndexAndType( configIdx, eArtifactComponentType.BLADE )
	ItemFlavor bladeComponent = LoadoutSlot_GetItemFlavor( playerEHI, bladeEntry )

	LoadoutEntry powerSourceEntry   = Artifacts_Loadouts_GetEntryForConfigIndexAndType( configIdx, eArtifactComponentType.POWER_SOURCE )
	ItemFlavor powerSourceComponent = LoadoutSlot_GetItemFlavor( playerEHI, powerSourceEntry )

	LoadoutEntry deathboxEntry   = Artifacts_Loadouts_GetEntryForConfigIndexAndType( configIdx, eArtifactComponentType.DEATHBOX )
	ItemFlavor deathboxComponent = LoadoutSlot_GetItemFlavor( playerEHI, powerSourceEntry )

	LoadoutEntry themeEntry   = Artifacts_Loadouts_GetEntryForConfigIndexAndType( configIdx, eArtifactComponentType.THEME )
	ItemFlavor themeComponent = LoadoutSlot_GetItemFlavor( playerEHI, themeEntry )

	LoadoutEntry activationEmoteEntry   = Artifacts_Loadouts_GetEntryForConfigIndexAndType( configIdx, eArtifactComponentType.ACTIVATION_EMOTE )
	ItemFlavor activationEmoteComponent = LoadoutSlot_GetItemFlavor( playerEHI, powerSourceEntry )

	ArtifactConfig artifactConfig

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


















void function ServerCallback_Artifacts_SetPlayerArtifactViewmodelData( entity player, int bladeGUID, int powerSourceGUID )
{
	if ( player.p.artifactConfig == null )
	{
		ArtifactConfig artifactConfig
		artifactConfig.blade       = GetItemFlavorByGUID( bladeGUID )
		artifactConfig.powerSource = GetItemFlavorByGUID( powerSourceGUID )
		player.p.artifactConfig    = artifactConfig
	}
	else
	{
		ArtifactConfig artifactConfig = expect ArtifactConfig( player.p.artifactConfig )
		artifactConfig.blade          = GetItemFlavorByGUID( bladeGUID )
		artifactConfig.powerSource    = GetItemFlavorByGUID( powerSourceGUID )
	}
}

ArtifactViewmodelData ornull function ClientCodeCallback_GetArtifactViewmodelDataForWeapon( entity weapon )
{
	if ( !weapon.GetWeaponSettingBool( eWeaponVar.is_artifact ) )
		return null

	entity owner = weapon.GetOwner()
	if ( !IsValid(owner) )
	{
		Warning( "Code called GetArtifactViewmodelDataForWeapon on a weapon that has no owner\n")
		return null
	}

	Assert( owner.IsBot() || owner.p.artifactConfig != null )
	if ( owner.p.artifactConfig == null )
		return null

	ArtifactConfig artifactConfig = expect ArtifactConfig( owner.p.artifactConfig )

	ArtifactViewmodelData res
	res.bladeGUID = ItemFlavor_GetGUID( artifactConfig.blade )
	res.powerSourceGUID = ItemFlavor_GetGUID( artifactConfig.powerSource )

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

	weapon.kv.rendercolor = Artifacts_FX_GetSmearColor( artifactConfig.powerSource )



















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

void function Artifacts_Loadouts_PreviewBladeAndPowerSource( entity weapon, ItemFlavor blade, ItemFlavor powerSource )
{
	Assert( IsLobby() )

	if ( Artifacts_IsEmptyComponent( blade ) )
		return

	_PreviewComponent( weapon, eArtifactSetIndex[ Artifacts_GetSetKey( blade ) ], BLADE_BODY_GROUP_MOD_PREFIX )

	
	int componentIdx = Artifacts_IsEmptyComponent( powerSource ) ? eArtifactSetIndex.COUNT : eArtifactSetIndex[ Artifacts_GetSetKey( powerSource ) ]
	_PreviewComponent( weapon, componentIdx, POWER_SOURCE_BODY_GROUP_MOD_PREFIX )

	foreach ( int fxHandle in fileLevel.lobbyFxHandles )
	{
		if ( EffectDoesExist( fxHandle ) )
			EffectStop( fxHandle, false, true )
	}

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

	weapon.kv.rendercolor = Artifacts_FX_GetSmearColor( powerSource )
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

ArtifactFX1PAnd3P function Artifacts_FX_Get1PAnd3PFXArraysFromPlayerLoadouts( entity player, int fxType )
{
	if ( player.p.artifactConfig == null )
	{
		ArtifactFX1PAnd3P fx1P3P
		return fx1P3P
	}

	ArtifactConfig artifactConfig = expect ArtifactConfig( player.p.artifactConfig )

	return Artifacts_FX_Get1PAnd3PFXArraysFromPowerSource( artifactConfig.powerSource, fxType )
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

		string impactTable = Artifacts_FX_GetImpactTable( powerSource ) 
		if ( impactTable != "" )
			PrecacheImpactEffectTable( impactTable )
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

vector function Artifacts_FX_GetSmearColor( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )
	var settingsBlock = ItemFlavor_GetSettingsBlock( powerSource )
	return ( GetSettingsBlockVector( settingsBlock, FX_SMEAR_COLOR ) * 255 )
}

int function Artifacts_FX_GetSkinIndex( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )
	var settingsBlock = ItemFlavor_GetSettingsBlock( powerSource )
	return GetSettingsBlockInt( settingsBlock, FX_SKIN_INDEX )
}

string function Artifacts_FX_GetImpactTable( ItemFlavor powerSource )
{
	Assert( ItemFlavor_GetType( powerSource ) == eItemType.artifact_component_power_source )

	string impactTable = GetSettingsBlockString( ItemFlavor_GetSettingsBlock( powerSource ), FX_IMPACT_TABLE )
	return impactTable
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


void function Artifact_Set1pFxControlPoints( entity weapon, int effectHandle )
{
	if ( IsValid( weapon && weapon.IsWeaponX() ) )
	{
		entity owner = weapon.GetOwner()

		ArtifactFX1PAnd3P fx1P3P = Artifacts_FX_Get1PAnd3PFXArraysFromPlayerLoadouts( owner, eArtifactFXPackageType.IDLE )
		int fxLen = maxint( fx1P3P.fx1P.len(), fx1P3P.fx1P.len() )
		for ( int i = 0; i < fxLen; i++ )
		{
			asset onePAsset   = fx1P3P.fx1P.isvalidindex( i ) ? fx1P3P.fx1P[ i ].fxAsset : $""
			string attachName = fx1P3P.fx1P.isvalidindex( i ) ? fx1P3P.fx1P[ i ].attachName : fx1P3P.fx3P[ i ].attachName
			Assert( attachName != "" )
			asset threePAsset = fx1P3P.fx3P.isvalidindex( i ) ? fx1P3P.fx3P[ i ].fxAsset : $""

			entity viewmodel = null
			if ( owner.GetActiveWeapon( eActiveInventorySlot.mainHand ) == weapon )
				viewmodel = owner.GetViewModelEntity()

			if ( IsValid( viewmodel ) && EffectDoesExist( effectHandle ) )
			{
				if ( owner.p.artifactConfig == null )
					return

				ArtifactConfig artifactConfig = expect ArtifactConfig( owner.p.artifactConfig )

				array< ArtifactFXControlPoint > controlPoints = Artifacts_FX_GetBladeControlPoints( artifactConfig.blade, false )
				foreach ( ArtifactFXControlPoint cp in controlPoints )
					EffectAddTrackingForControlPoint( effectHandle, cp.controlPointNumber, viewmodel, FX_PATTACH_POINT_FOLLOW, viewmodel.LookupAttachment( attachName ), cp.controlPoint )
			}

		}
	}
}



table< int, array< FXHandle > > s_fxHandles


void function Artifacts_FX_StartWeaponFX( entity weapon, int fxPackageType, entity playerOwner )
{
	int weaponEffect		 = ARTIFACT_FX_HANDLE_INVALID
	entity effectEntity		 = null
	bool isWeaponEnt		 = weapon.IsWeaponX()
	entity player            = playerOwner
	if ( !IsValid( player ) )
	{
		Assert( isWeaponEnt )
		player = weapon.GetWeaponOwner()
	}

	
	if ( fxPackageType == eArtifactFXPackageType.BLADE_EMISSIVE )
	{
		if ( player.p.artifactConfig == null )
			return

		ArtifactConfig artifactConfig = expect ArtifactConfig( player.p.artifactConfig )
		weapon.kv.rendercolor = Artifacts_FX_GetSmearColor( artifactConfig.powerSource )
		return
	}

	ArtifactFX1PAnd3P fx1P3P = Artifacts_FX_Get1PAnd3PFXArraysFromPlayerLoadouts( player, fxPackageType )
	int fxLen = maxint( fx1P3P.fx1P.len(), fx1P3P.fx1P.len() )
	for ( int i = 0; i < fxLen; i++ )
	{
		asset onePAsset   = fx1P3P.fx1P.isvalidindex( i ) ? fx1P3P.fx1P[ i ].fxAsset : $""
		string attachName = fx1P3P.fx1P.isvalidindex( i ) ? fx1P3P.fx1P[ i ].attachName : fx1P3P.fx3P[ i ].attachName
		Assert( attachName != "" )
		asset threePAsset = fx1P3P.fx3P.isvalidindex( i ) ? fx1P3P.fx3P[ i ].fxAsset : $""

		if ( fxPackageType == eArtifactFXPackageType.FLOURISH ) 
		{

				entity weaponVm = weapon.GetWeaponViewmodel()
				int fxHandle = StartParticleEffectOnEntity( weaponVm, GetParticleSystemIndex( onePAsset ), FX_PATTACH_POINT_FOLLOW, weaponVm.LookupAttachment( attachName ) )
				if ( !( fxPackageType in s_fxHandles ) )
					s_fxHandles[ fxPackageType ] <- []
				s_fxHandles[ fxPackageType ].append( fxHandle )



		}
		else
		{





				weapon.PlayWeaponEffect( onePAsset, threePAsset, attachName, true )
		}
	}
}

void function Artifacts_FX_StopWeaponFX( entity weapon, int fxPackageType )
{
	
	if ( fxPackageType == eArtifactFXPackageType.BLADE_EMISSIVE )
	{
		weapon.kv.rendercolor = "0 0 0"
		return
	}


		if ( fxPackageType == eArtifactFXPackageType.FLOURISH && fxPackageType in s_fxHandles )
		{
			array< FXHandle > allFxHandles = clone ( s_fxHandles[ fxPackageType ] )
			foreach ( FXHandle handle in allFxHandles )
			{
				if ( EffectDoesExist( handle ) )
					EffectStop( handle, false, true )
			}
			delete s_fxHandles[ fxPackageType ]
			return
		}


	entity player            = weapon.GetWeaponOwner()
	ArtifactFX1PAnd3P fx1P3P = Artifacts_FX_Get1PAnd3PFXArraysFromPlayerLoadouts( player, fxPackageType )
	int fxLen = maxint( fx1P3P.fx1P.len(), fx1P3P.fx1P.len() )
	for ( int i = 0; i < fxLen; i++ )
	{
		asset onePAsset   = ( fx1P3P.fx1P.isvalidindex( i ) && fxPackageType != eArtifactFXPackageType.FLOURISH ) ? fx1P3P.fx1P[ i ].fxAsset : $""
		asset threePAsset = fx1P3P.fx3P.isvalidindex( i ) ? fx1P3P.fx3P[ i ].fxAsset : $""
		weapon.StopWeaponEffect( onePAsset, threePAsset )
	}
}

void function Artifacts_FX_ScriptAnimWindowCallback( entity weapon, string parameter, bool isWindowStart )
{
	entity owner = weapon.GetOwner()

	Assert( owner.IsBot() || owner.p.artifactConfig != null )
	if ( owner.p.artifactConfig == null )
		return

	ArtifactConfig artifactConfig = expect ArtifactConfig( owner.p.artifactConfig )

	if ( Artifacts_IsEmptyComponent( artifactConfig.powerSource ) )
		return

	int setIndex = Artifacts_GetSetIndex( artifactConfig.powerSource )

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
						Artifacts_FX_StartWeaponFX( weapon, fxPackageType, null )
					else
						Artifacts_FX_StopWeaponFX( weapon, fxPackageType )

					break
				}
			}

			break
		}
	}
}

void function Artifacts_PlayActivationEmote( entity weapon )
{





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

	Artifacts_StoreLoadoutDataOnPlayerEntityStruct( player )
}






























































































































































































































void function Artifacts_DEV_UnitTest_PowerSource( ItemFlavor powerSource )
{
	Artifacts_FX_GetIdleFX( powerSource )
	Artifacts_FX_GetFlourishFX( powerSource )
	Artifacts_FX_GetAttackFX( powerSource )
	Artifacts_FX_GetStartupFX( powerSource )
	Artifacts_FX_GetSmearColor( powerSource )
	Artifacts_FX_GetSkinIndex( powerSource )
	Artifacts_FX_GetImpactTable( powerSource )
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
