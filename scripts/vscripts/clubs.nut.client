global function Clubs_Init























































global function GetAllClubLogoElementFlavors
global function GetAllClubLogoElementFlavorsOfType
global function GetRandomClubLogoElementFlavor
global function ClubLogo_GetLogoElementImage
global function ClubLogo_GetLogoSecondaryColorMask
global function ClubLogo_GetLogoFrameMask
global function ClubLogo_GetLogoElementName
global function ClubLogo_GetLogoElementType
global function ClubLogo_GetLogoVerticalOffset
global function ClubLogo_GetLogoColorTable
global function ClubLogo_GetColorSwatchCount

global function GenerateRandomClubLogo
global function ClubLogo_ConvertLogoToString
global function ClubLogo_ConvertLogoStringToLogo






global function ClubSearchTag_GetAllEnabledSearchTags
global function ClubSearchTag_CreateSearchTagBitMask
global function ClubSearchTag_GetItemFlavorFromBitMaskAddress
global function ClubSearchTag_GetItemFlavorsFromBitMask
global function ClubSearchTag_GetTagString
global function ClubSearchTag_GetTagType
global function ClubSearchTag_GetSearchTagNamesFromBitmask











global function Clubs_AreObituaryTagsEnabledByPlaylist




























global function ClubRegulation_GetReasonString






global function Clubs_IsEnabled
global function Clubs_AreDisabledByPlaylist






global function Clubs_UpdateCrossplayVar
global function Clubs_UIToClient_SetCrossplayVar
global function Clubs_SetMyStoredClubName
global function Clubs_GetMyStoredClubName


























































global const int CLUB_QUERY_RETRY_MAX = 5
global const string CLUB_REQUERY_SIGNAL = "ClubAttemptRequery"

global const string CLUB_UPDATE_TAB_SIGNAL = "ClubUpdateTabSignal"

global const int CLUB_LOGO_LAYER_MAX = 3
global const int CLUB_LOGO_LAYER_PROPERTY_COUNT = 3
global const string CLUB_LOGO_LAYER_DELIMITER = ";"
global const string CLUB_LOGO_PROPERTY_DELIMITER = ","
global const string CLUB_LOGO_COLORVEC_DELIMITER = "_"
global const string CLUB_EVENT_DELIMITER = "%"
global const float CLUB_LOGO_ROTATION_STEP = 45.0
global const int CLUB_SEARCH_MAX_RESULTS = 50
global const int CLUB_SEARCH_TAG_SELECTION_MAX = 5
global const int CLUB_TAG_LENGTH_MIN = 3
global const int CLUB_NAME_LENGTH_MIN = 4
global const int CLUB_ANNOUNCE_COOLDOWN_MINUTES = 15
global const int CLUB_JOIN_MIN_ACCOUNT_LEVEL = 9

global const string INVALID_CLUB_ID = ""
global const string PENDING_CLUB_REQUEST = "PendingClubRequest"
global const int INVALID_ANNOUNCE_VIEW_TIME = -1

global const array<string> ILLEGAL_CHAT_CHARS = ["%", "`"]

const int CLUB_MAX_NAME_LENGTH_FOR_CHAT = 16

global enum eClubLogoElementType
{
	CLUBLOGOTYPE_FRAME,
	CLUBLOGOTYPE_EMBLEM,
	CLUBLOGOTYPE_BACKGROUNDS,

	_count
}

global struct ClubLogoLayer
{
	ItemFlavor& elementFlav
	int			elementType
	vector      pos = <0.0,0.0,0.0>
	vector      scale = <1.0,1.0,0>
	float       rotation = 0.0
	vector      primaryColorOverride = <255,255,255>
	float		verticalOffset = 0.0
	vector      secondaryColorOverride = <255,255,255>
	float       secondaryColorAlpha = 1.0
	asset		frameMask
}

global struct ClubLogo
{
	array<ClubLogoLayer> logoLayers
	bool isInvite = false
}

struct ClubHSV
{
	float hue
	float saturation
	float value
}

global enum eClubMinAccountLevel
{
	MINLVL_10,
	MINLVL_50,
	MINLVL_100,
	MINLVL_200,
	MINLVL_300,
	MINLVL_400,
	MINLVL_500,

	_count
}

global enum eClubMinRank
{
	MINRANK_BRONZE,
	MINRANK_SILVER,
	MINRANK_GOLD,
	MINRANK_PLATINUM,
	MINRANK_DIAMOND,
	MINRANK_MASTER,
	MINRANK_APEXPREDATOR,

	_count
}


global enum eClubInviteDisplayLevel
{
	DISABLED,
	ENABLED,

	_count
}


global enum eClubSearchTagFlags
{
	
	MODES_RANKED = (1 << 0)
	MODES_UNRANKED = (1 << 1)
	MODES_DUOS = (1 << 2)



	MODES_ANY = (1 << 4)

	
	PLAYSTYLE_COMPETITIVE = (1 << 5)
	PLAYSTYLE_CASUAL = (1 << 6)
	PLAYSTYLE_ALLSTYLES = (1 << 7)
	PLAYSTYLE_LONEWOLVES  = (1 << 8)
	PLAYSTYLE_TEAMPLAYERS = (1 << 9)

	
	EXP_BEGINNERS = (1 << 10)
	EXP_WILLHELPBEGINNERS = (1 << 11)
	EXP_EXPERIENCED = (1 << 12)
	EXP_ALLSKILLS = (1 << 13)

	
	COMMS_MIC_ONLY = (1 << 14)
	COMMS_MIC_OPTIONAL = (1 << 15)
	COMMS_MIC_NO = (1 << 16)

	
	SOC_FAMILY_FRIENDLY = (1 << 17)
	SOC_MATURE = (1 << 18)
	SOC_YOUNG = (1 << 19) 
	SOC_LGBT = (1 << 20)
	SOC_DISABLED_GAMERS = (1 << 21)
	SOC_SWEARING_OK = (1 << 22)
	SOC_SWEARING_NO = (1 << 23)
	SOC_TRASHTALK_OK = (1 << 24)
	SOC_TRASHTALK_NO = (1 << 25)
}

global enum eClubSearchTagType
{
	CLUBTAGTYPE_GAMEMODE,
	CLUBTAGTYPE_PLAYSTYLE,
	CLUBTAGTYPE_EXPERIENCE,
	CLUBTAGTYPE_COMMUNICATION,
	CLUBTAGTYPE_SOCIAL,
	CLUBTAGTYPE_PLATFORM,

	_count
}


global enum eClubInternalReportReason
{
	REASON_CHAT_OFFENSIVE,
	REASON_CHAT_SPAM,
	REASON_CHAT_HARASSMENT,
	REASON_CHAT_HATESPEECH,
	REASON_CHAT_SUICIDETHREAT,
	_chat_count,

	REASON_GAME_OFFENSIVE,
	REASON_GAME_RUDETOCLUBMATES,
	REASON_GAME_RUDETORANDOMS,
	REASON_GAME_CHEATS,
	_count,
}

global enum eClubQueryState
{
	INACTIVE,
	PROCESSING,
	FAILED,
	SUCCESSFUL,
	_count,
}

global const string CLUBCMD_REPORT_PLACEMENT_ADDPLAYER = "ServerToUI_AddPlayerDataForPlacementReport"
global const string CLUBCMD_UPDATE_LAST_MATCH_TIME = "ServerToUI_Clubs_UpdateLastMatchTimes"

global struct ClubSquadSummaryPlayerData
{
	string nucleusID
	int kills
	int damageDealt
}

struct ClubSquadSummaryData
{
	array<ClubSquadSummaryPlayerData> playerData
	int                               squadplacement = -1
}

struct
{
	table< int, array<vector> > logoColorPalette

	int clubQueryRetryCount = 0


		string myClubName



		bool crossplayEnabled
		bool kickedForCrossplay






















} file

void function Clubs_Init()
{
	printf( "ClubsDebug: Init Clubs" )

	AddCallback_RegisterRootItemFlavors( void function() {
		foreach ( asset logoElementAsset in GetBaseItemFlavorsFromArray( "clubLogoElements" ) )
			RegisterItemFlavorFromSettingsAsset( logoElementAsset )
	} )

	AddCallback_RegisterRootItemFlavors( void function() {
		foreach ( asset searchTag in GetBaseItemFlavorsFromArray( "clubSearchTags" ) )
			RegisterItemFlavorFromSettingsAsset( searchTag )
	} )

	file.logoColorPalette = BuildColorSwatchTable()

	
	
	


		AddCallback_OnSettingsUpdated( OnSettingsUpdated )



		file.crossplayEnabled = GetConVarBool( "CrossPlay_user_optin" )















































}




















































































































































































































































































































































































































































































































































































































































































array<ItemFlavor> function GetAllClubLogoElementFlavors()
{
	return  GetAllItemFlavorsOfType( eItemType.club_logo_element )
}

array<ItemFlavor> function GetAllClubLogoElementFlavorsOfType( int elementType )
{
	array<ItemFlavor> logoElements = GetAllClubLogoElementFlavors()
	array<ItemFlavor> logoElementsOfType

	foreach ( ItemFlavor logoElement in logoElements )
	{
		if ( ClubLogo_GetLogoElementType( logoElement ) == elementType && IsClubLogoElementEnabled( logoElement ) )
			logoElementsOfType.append( logoElement )
	}

	return logoElementsOfType
}

ItemFlavor function GetRandomClubLogoElementFlavor()
{
	array<ItemFlavor> elementFlavors = GetAllClubLogoElementFlavors()
	return elementFlavors[ RandomIntRange( 0, (elementFlavors.len() - 1) ) ]
}

ItemFlavor function GetRandomClubLogoElementFlavorByType( int elementType, var seed = null )
{
	array<ItemFlavor> elementFlavors = GetAllClubLogoElementFlavorsOfType( elementType )
	printf( "GetRandomClubLogoElementFlavorByType: type: %i, elementFlavors.len() = %i", elementType, elementFlavors.len() )
	if ( seed )
		return elementFlavors.getrandomseeded( seed )
	return elementFlavors.getrandom()
}

bool function IsClubLogoElementEnabled( ItemFlavor element )
{
	Assert( ItemFlavor_GetType( element ) == eItemType.club_logo_element )
	return GetGlobalSettingsBool( ItemFlavor_GetAsset( element ), "clubLogoElementEnabled" )
}

asset function ClubLogo_GetLogoElementImage( ItemFlavor element )
{
	Assert( ItemFlavor_GetType( element ) == eItemType.club_logo_element )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( element ), "clubLogoElementImage" )
}

asset ornull function ClubLogo_GetLogoSecondaryColorMask( ItemFlavor element )
{
	Assert( ItemFlavor_GetType( element ) == eItemType.club_logo_element )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( element ), "clubLogoElementSecondaryColorMask" )
}

asset ornull function ClubLogo_GetLogoFrameMask( ItemFlavor element )
{
	Assert( ItemFlavor_GetType( element ) == eItemType.club_logo_element )
	return GetGlobalSettingsAsset( ItemFlavor_GetAsset( element ), "clubLogoElementFrameMask" )
}

float function ClubLogo_GetLogoVerticalOffset( ItemFlavor element )
{
	Assert( ItemFlavor_GetType( element ) == eItemType.club_logo_element )
	return GetGlobalSettingsFloat( ItemFlavor_GetAsset( element ), "elementCenterPointAdjustment" )
}

string function ClubLogo_GetLogoElementName( ItemFlavor element )
{
	Assert( ItemFlavor_GetType( element ) == eItemType.club_logo_element )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( element ), "localizationKey_NAME_SHORT" )
}

int function ClubLogo_GetLogoElementType( ItemFlavor element )
{
	Assert( ItemFlavor_GetType( element ) == eItemType.club_logo_element )
	return eClubLogoElementType[ GetGlobalSettingsString( ItemFlavor_GetAsset( element ), "elementCategory" ) ]
}

ClubLogo function GenerateRandomClubLogo( string clubID = "" )
{
	ClubLogo newLogo

	var seed = null
	if ( clubID != "" )
		seed = CreateRandomSeed( clubID.tointeger() )

	array<vector> colorSet = ClubLogo_GetRandomDoubleSplitComplementaryColorSet( seed )

	Assert( colorSet.len() == 5, "ClubLogo_GetRandomDoubleSplitComplementaryColorSet() acquired incorect number of colors" )

	for ( int i = 0; i < CLUB_LOGO_LAYER_MAX; i++ )
	{
		ClubLogoLayer newLayer

		newLayer.elementFlav = GetRandomClubLogoElementFlavorByType( i, seed )

		
		

		switch( i )
		{
			case eClubLogoElementType.CLUBLOGOTYPE_FRAME:
				newLayer.primaryColorOverride = colorSet[0]
				break
			case eClubLogoElementType.CLUBLOGOTYPE_EMBLEM:
				newLayer.primaryColorOverride = colorSet[1]
				newLayer.secondaryColorOverride = colorSet[2]
				break
			case eClubLogoElementType.CLUBLOGOTYPE_BACKGROUNDS:
				newLayer.primaryColorOverride = colorSet[3]
				newLayer.secondaryColorOverride = colorSet[4]
		}

		newLayer.verticalOffset = ClubLogo_GetLogoVerticalOffset( newLayer.elementFlav )

		newLogo.logoLayers.append( newLayer )
	}

	asset frameMask
	float verticalOffset
	array<ClubLogoLayer> nonFrameLayers
	foreach ( ClubLogoLayer layer in newLogo.logoLayers )
	{
		if ( ClubLogo_GetLogoElementType( layer.elementFlav ) == eClubLogoElementType.CLUBLOGOTYPE_FRAME )
		{
			frameMask = expect asset( ClubLogo_GetLogoFrameMask( layer.elementFlav ) )
			verticalOffset = ClubLogo_GetLogoVerticalOffset( layer.elementFlav )
		}
		else
		{
			nonFrameLayers.append( layer )
		}
	}

	foreach ( ClubLogoLayer layer in newLogo.logoLayers )
	{
		if ( nonFrameLayers.contains( layer ) )
			layer.frameMask = frameMask

		int layerType = ClubLogo_GetLogoElementType( layer.elementFlav )

		if ( layerType == eClubLogoElementType.CLUBLOGOTYPE_EMBLEM )
			layer.verticalOffset = verticalOffset
	}

	return newLogo
}

table< int, array<vector> > function BuildColorSwatchTable()
{
	table< int, array<vector> > swatchTable

	var dt       = GetDataTable( $"datatable/color_swatch_table.rpak" )
	int rowCount = GetDataTableRowCount( dt )

	array<vector> whiteToBlack
	int whiteCol = GetDataTableColumnByName( dt, "whiteToBlack" )
	for ( int i = 0; i < rowCount; i++ )
	{
		whiteToBlack.append( GetDataTableVector( dt, i, whiteCol ) )
	}

	array< array<vector> > allColors
	allColors.append( whiteToBlack )

	for ( int valueIdx = 0; valueIdx < rowCount; valueIdx++ )
	{
		array<vector> colorRow
		for( int hueIdx = 0; hueIdx < rowCount; hueIdx++ )
		{
			int hueRow = GetDataTableColumnByName( dt, format( "color%02d", hueIdx ) )
			vector newColor = GetDataTableVector( dt, valueIdx, hueRow )

			if ( newColor == <999,999,999> )
				continue
			else
				colorRow.append( newColor )
		}
		allColors.append( colorRow )
	}

	for ( int i = 0; i < allColors.len(); i++ )
	{
		swatchTable[ i ] <- allColors[i]
	}

	printf( "ClubsDebug: BuildColorSwatchTable: Built table of %i colors", ClubLogo_GetColorSwatchCount( swatchTable ) )
	return swatchTable
}

int function ClubLogo_GetColorSwatchCount( table< int, array<vector> > swatchTable )
{
	int colorCount
	foreach ( int colorIndex, array<vector> colorShades in swatchTable )
	{
		array<vector> shades = swatchTable[ colorIndex ]
		for ( int i = 0; i < shades.len(); i++ )
			colorCount++
	}

	return colorCount
}

table< int, array<vector> > function ClubLogo_GetLogoColorTable()
{
	return file.logoColorPalette
}

vector function ClubLogo_GetRandomLogoColor( var seed = null )
{
	vector color = <255.0,255.0,255.0>

	printf( "RandomLogoDebug: Picking a random color from %i options", ClubLogo_GetColorSwatchCount( file.logoColorPalette ) )

	int rowCount

	foreach( int colorRow, array<vector> colorArray in file.logoColorPalette )
	{
		if ( colorRow > rowCount )
			rowCount = colorRow
	}

	int randomRow
	if ( seed )
		randomRow = RandomIntRangeSeeded( seed, 0, rowCount )
	else
		randomRow = RandomIntRange( 0, rowCount )

	if ( file.logoColorPalette[ randomRow ].len() > 0 )
	{
		if ( seed )
			color = file.logoColorPalette[ randomRow ].getrandomseeded( seed )
		else
			color = file.logoColorPalette[ randomRow ].getrandom()
	}
	else
	{
		printf( "RandomLogoDebug: Returning white because row %i is empty", randomRow )
	}

	printf( "RandomLogoDebug: Returning <%f, %f, %f>", color.x, color.y, color.z )
	return color
}

const float RANDOMLOGO_HUE_SHIFT_DEGREES = 30.0
const float RANDOMLOGO_VALUE_SHIFT_PERCENT = 0.25
array<vector> function ClubLogo_GetRandomDoubleSplitComplementaryColorSet( var seed = null )
{
	vector startingColor = ClubLogo_GetRandomLogoColor( seed )
	bool isShadeOfGray = ( startingColor.x == startingColor.y && startingColor.x == startingColor.z )

	ClubHSV startingHSV       = RGBToHSV( startingColor )

	ClubHSV splitComplement01 = clone( startingHSV )
	splitComplement01.hue = HSVHueShift( splitComplement01.hue, 180.0 - RANDOMLOGO_HUE_SHIFT_DEGREES )
	splitComplement01.value = isShadeOfGray ? HSVSaturationOrValueShift( 1-splitComplement01.value, RANDOMLOGO_VALUE_SHIFT_PERCENT ) : splitComplement01.value

	ClubHSV splitComplement02 = clone( startingHSV )
	splitComplement02.hue = HSVHueShift( splitComplement02.hue, 180.0 + RANDOMLOGO_HUE_SHIFT_DEGREES )
	splitComplement02.value = isShadeOfGray ? HSVSaturationOrValueShift( 1-splitComplement02.value, -RANDOMLOGO_VALUE_SHIFT_PERCENT ) : splitComplement02.value

	ClubHSV dblComplement01 = clone( splitComplement01 )
	dblComplement01.hue = HSVHueShift( dblComplement01.hue, 180.0 )
	dblComplement01.value = isShadeOfGray ? HSVSaturationOrValueShift( dblComplement01.value, RANDOMLOGO_VALUE_SHIFT_PERCENT/2 ) : dblComplement01.value
	ClubHSV dblComplement02 = clone( splitComplement02 )
	dblComplement02.hue = HSVHueShift( dblComplement02.hue, 180.0 )
	dblComplement02.value = isShadeOfGray ? HSVSaturationOrValueShift( dblComplement02.value, -RANDOMLOGO_VALUE_SHIFT_PERCENT/2 ) : dblComplement02.value

	array<vector> colorSet = [
		startingColor,
		FindNearestColorOnColorTable( HSVToRGB( splitComplement01 ) ),
		FindNearestColorOnColorTable( HSVToRGB( splitComplement02 ) ),
		FindNearestColorOnColorTable( HSVToRGB( dblComplement01 ) ),
		FindNearestColorOnColorTable( HSVToRGB( dblComplement02 ) ),
	]

	return colorSet
}

vector function FindNearestColorOnColorTable( vector color )
{
	vector closestColor
	float closestDistToColor = -1

	foreach ( int rowIdx, array<vector> colorEntries in file.logoColorPalette )
	{
		foreach ( vector colorEntry in colorEntries )
		{
			float distToColor = sqrt( pow( (color.x - colorEntry.x), 2.0 ) + pow( (color.y - colorEntry.y), 2.0 ) + pow( (color.z - colorEntry.z), 2.0 ) )

			if ( distToColor < closestDistToColor || closestDistToColor < 0.0 )
			{
				closestColor = colorEntry
				closestDistToColor = distToColor
			}
		}
	}

	return closestColor
}

ClubHSV function RGBToHSV( vector rgb )
{
	ClubHSV hsv
	float maxChannel = max( rgb.x, rgb.y )
	maxChannel = max( maxChannel, rgb.z )
	float minChannel = min( rgb.x, rgb.y )
	minChannel = min( minChannel, rgb.z )
	float difference = maxChannel - minChannel

	hsv.saturation = (maxChannel == 0.0) ? 0.0 : (difference/maxChannel)

	if ( hsv.saturation == 0 )
		hsv.hue = 0
	else if ( maxChannel == rgb.x )
		hsv.hue = 60.0 * ( rgb.y - rgb.z ) / difference
	else if ( maxChannel == rgb.y )
		hsv.hue = 120.0 + 60.0 * ( rgb.z - rgb.x ) / difference
	else if ( maxChannel == rgb.z )
		hsv.hue = 240.0 + 60.0 * ( rgb.x - rgb.y ) / difference

	if ( hsv.hue < 0.0 )
		hsv.hue += 360.0

	hsv.hue = RoundFloat( hsv.hue )
	hsv.value = maxChannel/255.0

	return hsv
}

float function HSVHueShift( float hue, float shiftDegrees )
{
	float newHue = hue + shiftDegrees

	while ( newHue >= 360.0 )
		newHue -= 360.0

	while ( newHue < 0.0 )
		newHue += 360.0

	return newHue
}

float function HSVSaturationOrValueShift( float value, float shiftPercent )
{
	float newValue = value + shiftPercent

	while ( newValue >= 1.0 )
		newValue -= 1.0

	while ( newValue < 0.0 )
		newValue += 1.0

	return newValue
}

vector function HSVToRGB( ClubHSV hsv )
{
	vector rgb
	ClubHSV adjustedHSV = hsv

	if ( hsv.saturation == 0.0 )
	{
		rgb.x = RoundFloat( hsv.value * 255.0 )
		rgb.y = RoundFloat( hsv.value * 255.0 )
		rgb.z = RoundFloat( hsv.value * 255.0 )
		return rgb
	}

	adjustedHSV.hue /= 60.0
	float i = floor( adjustedHSV.hue )
	float f = adjustedHSV.hue - i
	float p = adjustedHSV.value * ( 1 - adjustedHSV.saturation )
	float q = adjustedHSV.value * ( 1 - adjustedHSV.saturation * f )
	float t = adjustedHSV.value * ( 1 - adjustedHSV.saturation * (1-f) )
	switch ( i )
	{
		case 0:
			rgb.x = adjustedHSV.value
			rgb.y = t
			rgb.z = p
			break
		case 1:
			rgb.x = q
			rgb.y = adjustedHSV.value
			rgb.z = p
			break
		case 2:
			rgb.x = p
			rgb.y = adjustedHSV.value
			rgb.z = t
			break
		case 3:
			rgb.x = p
			rgb.y = q
			rgb.z = adjustedHSV.value
			break
		case 4:
			rgb.x = t
			rgb.y = p
			rgb.z = adjustedHSV.value
			break
		default:
			rgb.x = adjustedHSV.value
			rgb.y = p
			rgb.z = q
	}

	rgb.x = RoundFloat( rgb.x * 255.0 )
	rgb.y = RoundFloat( rgb.y * 255.0 )
	rgb.z = RoundFloat( rgb.z * 255.0 )

	return rgb
}

float function RoundFloat( float number )
{
	float integral = floor( number )
	float decimal  = fabs( number % 1.0 )

	if ( decimal >= 0.5 )
		integral += signum( integral )

	return integral
}

string function ClubLogo_ConvertLogoToString( ClubLogo logo )
{
	string clubLogoString

	for ( int layerIndex = 0; layerIndex < logo.logoLayers.len(); layerIndex++ )
	{
		ClubLogoLayer layer = logo.logoLayers[layerIndex]

		string nameString    = ItemFlavor_GetGUIDString( layer.elementFlav ) + CLUB_LOGO_PROPERTY_DELIMITER
		string primaryColorX = string( layer.primaryColorOverride.x ) + CLUB_LOGO_COLORVEC_DELIMITER
		string primaryColorY = string( layer.primaryColorOverride.y ) + CLUB_LOGO_COLORVEC_DELIMITER
		string primaryColorZ = string( layer.primaryColorOverride.z ) + CLUB_LOGO_PROPERTY_DELIMITER
		string secondaryColorX = string( layer.secondaryColorOverride.x ) + CLUB_LOGO_COLORVEC_DELIMITER
		string secondaryColorY = string( layer.secondaryColorOverride.y ) + CLUB_LOGO_COLORVEC_DELIMITER
		string secondaryColorZ = string( layer.secondaryColorOverride.z ) + CLUB_LOGO_LAYER_DELIMITER
		string secondaryColorAlpha = string( layer.secondaryColorAlpha ) + CLUB_LOGO_LAYER_DELIMITER

		clubLogoString = clubLogoString + nameString + primaryColorX + primaryColorY + primaryColorZ + secondaryColorX + secondaryColorY + secondaryColorZ
	}

	printf( "ClubLogoDebug: %s(): Converted logo to string: %s", FUNC_NAME(), clubLogoString )
	return clubLogoString
}

ClubLogo function ClubLogo_ConvertLogoStringToLogo( string logoString )
{
	array<string> logoStringArray = split( logoString, CLUB_LOGO_LAYER_DELIMITER )
	ClubLogo clubLogo

	if ( logoStringArray.len() == 0 )
	{
		return clubLogo
	}
	else if ( logoStringArray.len() < CLUB_LOGO_LAYER_MAX )
	{
		return clubLogo
	}
	else
	{
		if ( logoStringArray.len() > CLUB_LOGO_LAYER_MAX )
			logoStringArray.resize( CLUB_LOGO_LAYER_MAX )
	}

	foreach ( string layerString in logoStringArray )
	{
		int layerIndex = logoStringArray.find( layerString )

		ClubLogoLayer layer

		array<string> layerPropArray = split( layerString, CLUB_LOGO_PROPERTY_DELIMITER )

		string layerFlavGuidString = layerPropArray[0]

		
		SettingsAssetGUID layerFlavGUID = ConvertItemFlavorGUIDStringToGUID( layerFlavGuidString )
		if ( !IsValidItemFlavorGUID( layerFlavGUID ) )
		{
			Warning( "Club Logo Warning: Had to bypass adding ItemFlavor GUID " + layerFlavGuidString + " (layer index: " + layerIndex + ") to the logo as it is invalid." )
			continue
		}

		ItemFlavor ornull layerFlav = GetItemFlavorByGUID( layerFlavGUID )
		Assert( layerFlav != null, format( "Club Logo String returned null Item Flavor: %s", layerFlavGuidString ) )
		expect ItemFlavor( layerFlav )

		layer.elementFlav = layerFlav

		string layerPrimaryColorOverrideString = layerPropArray[1]
		array<string> primaryColorStringArray  = split( layerPrimaryColorOverrideString, CLUB_LOGO_COLORVEC_DELIMITER )
		Assert( primaryColorStringArray.len() == 3, "Club Logo String returned incorrect number of parameters for color vector" )
		vector colorOverride
		colorOverride.x = float( primaryColorStringArray[0] )
		colorOverride.y = float( primaryColorStringArray[1] )
		colorOverride.z = float( primaryColorStringArray[2] )
		layer.primaryColorOverride = colorOverride

		string layerSecondaryColorOverrideString = layerPropArray[2]
		array<string> secondaryColorStringArray = split( layerSecondaryColorOverrideString, CLUB_LOGO_COLORVEC_DELIMITER )
		if ( secondaryColorStringArray.len() == 3 )
		{
			vector secondaryColorOverride
			secondaryColorOverride.x = float( secondaryColorStringArray[0] )
			secondaryColorOverride.y = float( secondaryColorStringArray[1] )
			secondaryColorOverride.z = float( secondaryColorStringArray[2] )
			layer.secondaryColorOverride = secondaryColorOverride
		}
		else
		{
			layer.secondaryColorAlpha = 0.0
		}

		clubLogo.logoLayers.append( layer )
	}

	return clubLogo
}






























































array<ItemFlavor> function GetAllClubSearchTags()
{
	return GetAllItemFlavorsOfType( eItemType.club_search_tag )
}

array<ItemFlavor> function ClubSearchTag_GetAllEnabledSearchTags()
{
	array<ItemFlavor> tagFlavors = GetAllClubSearchTags()
	foreach ( ItemFlavor tagFlavor in tagFlavors )
	{
		if ( !ClubSearchTag_IsTagEnabled( tagFlavor ) )
			tagFlavors.removebyvalue( tagFlavor )
	}

	return tagFlavors
}

ItemFlavor ornull function ClubSearchTag_GetItemFlavorFromBitMaskAddress( int address )
{
	array<ItemFlavor> allTags = ClubSearchTag_GetAllEnabledSearchTags()

	foreach ( ItemFlavor searchTag in allTags )
	{
		int tagAddress = ClubSearchTag_GetBitFlag( searchTag )
		if ( tagAddress == address )
			return searchTag
	}

	return null
}

array<ItemFlavor> function ClubSearchTag_GetItemFlavorsFromBitMask( int flagMask )
{
	array<ItemFlavor> searchTags

	for ( int i = 0; i <= 31; i++ )
	{
		int mask = ( 1 << i )

		bool isFlagged = (flagMask & mask) == mask
		if ( isFlagged )
		{
			ItemFlavor ornull tagFlavor = ClubSearchTag_GetItemFlavorFromBitMaskAddress( i )
			if ( tagFlavor != null )
			{
				expect ItemFlavor( tagFlavor )
				printf( "ClubCreateDebug: Adding %s to detail array", ClubSearchTag_GetTagString( tagFlavor ) )
				searchTags.append( tagFlavor )
			}
		}
	}

	return searchTags
}

array<string> function ClubSearchTag_GetSearchTagNamesFromBitmask( int flagMask )
{
	array<string> searchTagNames

	array<ItemFlavor> searchTagFlavs = ClubSearchTag_GetItemFlavorsFromBitMask( flagMask )
	foreach ( ItemFlavor tagFlav in searchTagFlavs )
	{
		string tagName = ClubSearchTag_GetTagString( tagFlav )
		searchTagNames.append( tagName )
	}

	return searchTagNames
}

int function ClubSearchTag_CreateSearchTagBitMask( array<ItemFlavor> searchTags )
{
	

	int searchTagBitMask
	array<int> searchTagFlags
	foreach ( ItemFlavor searchTag in searchTags )
	{
		int flag = ClubSearchTag_GetBitFlag( searchTag )
		
		searchTagFlags.append( flag )
	}

	foreach ( int flag in searchTagFlags )
	{
		int address = ( 1 << flag )
		searchTagBitMask = searchTagBitMask | address
	}

	
	return searchTagBitMask
}

int function ClubSearchTag_GetBitFlag( ItemFlavor searchTag )
{
	Assert( ItemFlavor_GetType( searchTag ) == eItemType.club_search_tag )
	return GetGlobalSettingsInt( ItemFlavor_GetAsset( searchTag ), "tagBitAddress" )
}

int function ClubSearchTag_GetTagType( ItemFlavor searchTag )
{
	Assert( ItemFlavor_GetType( searchTag ) == eItemType.club_search_tag )
	return eClubSearchTagType[ GetGlobalSettingsString( ItemFlavor_GetAsset( searchTag ), "tagCategory" ) ]
}

string function ClubSearchTag_GetTagString( ItemFlavor searchTag )
{
	Assert( ItemFlavor_GetType( searchTag ) == eItemType.club_search_tag )
	return GetGlobalSettingsString( ItemFlavor_GetAsset( searchTag ), "localizationKey_NAME_SHORT" )
}

bool function ClubSearchTag_IsTagEnabled( ItemFlavor searchTag )
{
	Assert( ItemFlavor_GetType( searchTag ) == eItemType.club_search_tag )
	return GetGlobalSettingsBool( ItemFlavor_GetAsset( searchTag ), "isSearchTagEnabled" )
}

array<ItemFlavor> function ClubSearchTag_GetTagsByType( int tagType )
{
	array<ItemFlavor> flavors = ClubSearchTag_GetAllEnabledSearchTags()

	foreach( flavor in flavors )
	{
		if ( ClubSearchTag_GetTagType( flavor ) != tagType )
			flavors.removebyvalue( flavor )
	}

	return flavors
}






























































































bool function Clubs_AreObituaryTagsEnabledByPlaylist()
{
	return GetCurrentPlaylistVarBool( "ClubTagsInObituaryEnabled", true )
}







































const int MAX_PLACEMENT_FOR_CLUB_EVENT = 5
const int MAX_PLACEMENT_FOR_ARENAS_CLUB_EVENT = 1
const int MAX_PLACEMENT_FOR_WINTER_EXPRESS_CLUB_EVENT = 1
const int MAX_PLACEMENT_FOR_CONTROL_CLUB_EVENT = 1
const int MAX_PLACEMENT_FOR_FREEDM_CLUB_EVENT = 1





























































































































































































































































































































string function ClubRegulation_GetReasonString( int reasonInt )
{
	string reasonString
	switch ( reasonInt )
	{
		case eClubInternalReportReason.REASON_CHAT_OFFENSIVE:
			reasonString = "#REPORT_CLUB_INTERNAL_CAT_CHAT_OFFENSIVE"
			break
		case eClubInternalReportReason.REASON_CHAT_SPAM:
			reasonString = "#REPORT_CLUB_INTERNAL_CAT_CHAT_SPAM"
			break
		case eClubInternalReportReason.REASON_CHAT_HARASSMENT:
			reasonString = "#REPORT_CLUB_INTERNAL_CAT_CHAT_HARASSMENT"
			break
		case eClubInternalReportReason.REASON_CHAT_HATESPEECH:
			reasonString = "#REPORT_CLUB_INTERNAL_CAT_CHAT_HATESPEECH"
			break
		case eClubInternalReportReason.REASON_CHAT_SUICIDETHREAT:
			reasonString = "#REPORT_CLUB_INTERNAL_CAT_CHAT_SUICIDETHREAT"
			break
		case eClubInternalReportReason.REASON_GAME_CHEATS:
			reasonString = "#REPORT_CLUB_INTERNAL_CAT_GAME_CHEATS"
			break
		case eClubInternalReportReason.REASON_GAME_OFFENSIVE:
			reasonString = "#REPORT_CLUB_INTERNAL_CAT_GAME_OFFENSIVE"
			break
		case eClubInternalReportReason.REASON_GAME_RUDETOCLUBMATES:
			reasonString = "#REPORT_CLUB_INTERNAL_CAT_GAME_RUDECLUBMATES"
			break
		case eClubInternalReportReason.REASON_GAME_RUDETORANDOMS:
			reasonString = "#REPORT_CLUB_INTERNAL_CAT_GAME_RUDERANDOMS"
			break
		default:
			reasonString = "INVALID REPORT REASON"
			break
	}

	
	return reasonString
}





























bool function Clubs_IsEnabled()
{






	return false

}


bool function Clubs_AreDisabledByPlaylist()
{
	return GetCurrentPlaylistVarBool( "Clubs_ForceDisable", false )
}









































void function OnSettingsUpdated()
{
	bool crossplayEnabled = GetConVarBool( "CrossPlay_user_optin" )
	if ( crossplayEnabled != file.crossplayEnabled )
	{
		RunUIScript( "Clubs_OpenCrossplayChangeDialog" )
	}

	int localBool = crossplayEnabled ? 1 : 0
	int fileBool = file.crossplayEnabled ? 1 : 0
	int convarBool = GetConVarBool( "CrossPlay_user_optin" ) ? 1 : 0
	printf( "ClubsCrossplayDebug: crossplayEnabled: %i, file.crossplayEnabled: %i, convar: %i", localBool, fileBool, convarBool )
}

void function Clubs_UpdateCrossplayVar()
{
	file.crossplayEnabled = GetConVarBool( "CrossPlay_user_optin" )
}

void function Clubs_UIToClient_SetCrossplayVar( bool isEnabled )
{
	SetConVarBool( "CrossPlay_user_optin", isEnabled )
	Clubs_UpdateCrossplayVar()
}


void function Clubs_SetMyStoredClubName( string clubName )
{
	file.myClubName = clubName
}


string function Clubs_GetMyStoredClubName()
{
	return file.myClubName
}





































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































































