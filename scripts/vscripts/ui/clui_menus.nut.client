global function GetCharacterButtonRows

global function GetCharacterButtonRowsByRole
global function GetCharacterRoleSize

global function GetCharacterButtonRowSizes
global function LayoutCharacterButtons
global function SetNavUpDown
global function SetNav

global function LocalizeAndShortenNumber_Int
global function LocalizeAndShortenNumber_Float

global function IsTenThousandOrMore

#if DEV
global function DEV_UnitTestShortenNumber
#endif

const bool SHORTEN_NUMBER_DBG = false

struct RowData
{
	int xPos
	int yPos
	bool hasLeadingSpace
}

array< array<var> > function GetButtonRows( array<int> rowSizes, array<var> buttons )
{
	array< array<var> > buttonRows

	int buttonIndex = 0
	foreach ( rowSize in rowSizes )
	{
		array<var> row
		int last = buttonIndex + rowSize

		while ( buttonIndex < last )
		{
			row.append( buttons[buttonIndex] )
			buttonIndex++
		}

		buttonRows.append( row )
	}

	return buttonRows
}


array< array<var> > function GetCharacterButtonRows( array<var> buttons )
{
	array<int> rowSizes = GetCharacterButtonRowSizes( buttons.len() )

	return GetButtonRows(rowSizes, buttons)
}


array< array<var> > function GetCharacterButtonRowsByRole( array<var> buttons, array<ItemFlavor> orderedCharacter )
{
	array<int> rowSizes = GetCharacterRoleSize( orderedCharacter )

	return GetButtonRows(rowSizes, buttons)
}


array<int> function GetCharacterRoleSize( array<ItemFlavor> orderedCharacter )
{
	int offenseAmount = 0
	int skirmisherAmount = 0
	int reconAmount = 0
	int defenseAmount = 0
	int supportAmount = 0

	foreach ( character in orderedCharacter )
	{
		int characterRole = CharacterClass_GetRole(character)

		if (characterRole == 1)
		{
			offenseAmount++
		}
		else if (characterRole == 2)
		{
			skirmisherAmount++
		}
		else if (characterRole == 3)
		{
			reconAmount++
		}
		else if (characterRole == 4)
		{
			defenseAmount++
		}
		else if (characterRole == 5)
		{
			supportAmount++
		}
	}

	array<int> rowSizes
	if (offenseAmount > 0)
		rowSizes.append(offenseAmount)
	if (skirmisherAmount > 0)
		rowSizes.append(skirmisherAmount)
	if (reconAmount > 0)
		rowSizes.append(reconAmount)
	if (defenseAmount > 0)
		rowSizes.append(defenseAmount)
	if (supportAmount > 0)
		rowSizes.append(supportAmount)

	return rowSizes
}



array<int> function GetCharacterButtonRowSizes( int numButtons )
{
	Assert( numButtons > 0 )

	int numRows = 1
	if ( numButtons > 18 )
		numRows = 4
	else if ( numButtons > 10 )
		numRows = 3
	else if ( numButtons > 5 )
		numRows = 2

#if PC_PROG_NX_UI
	if( IsNxHandheldMode() && numButtons > 18 )
	{
		numRows = 3
	}
#endif
	int fullRowSize  = int( ceil( numButtons / float( numRows ) ) )
	int shortRowSize = fullRowSize - 1
	int remainingSlots = numButtons
	int numShortRows = 0

	while ( remainingSlots % fullRowSize > 0 )
	{
		numShortRows++
		remainingSlots -= shortRowSize
	}

	Assert( remainingSlots % fullRowSize == 0 )
	int numFullRows = remainingSlots / fullRowSize

	
	
	
	
	
	
	

	array<int> rowSizes
	bool prepend = false

	for ( int i = 0; i < numFullRows; i++ )
		rowSizes.append( fullRowSize )

	for ( int i = 0; i < numShortRows; i++ )
	{
		if ( i > 0 )
			prepend = !prepend

		if ( prepend )
			rowSizes.insert( 0, shortRowSize )
		else
			rowSizes.append( shortRowSize )
	}

	
	

	return rowSizes
}



void function LayoutCharacterButtons( array< array<var> > buttonRows )
{
	Assert( buttonRows.len() > 0 && buttonRows[0].len() > 0 )
	int buttonWidth        = Hud_GetWidth( buttonRows[0][0] )
	int buttonHeight       = Hud_GetHeight( buttonRows[0][0] )
	
	

	
	
	
	

	int spaceShift         = int( buttonWidth * 0.6237113402061856 )
	int rowShift

	rowShift           = 0




	
	

	array<RowData> rowData
	foreach ( row in buttonRows )
	{
		RowData data
		rowData.append( data )
	}

	foreach ( rIndex, row in buttonRows )
	{
		foreach ( bIndex, button in row )
		{
			
			if ( bIndex == 0 )
			{
				
				rowData[rIndex].yPos = buttonHeight * -1 * rIndex
			}
			else
			{
				Hud_SetPinSibling( button, Hud_GetHudName( row[bIndex - 1] ) )
				Hud_SetX( button, (spaceShift * -1) )
				Hud_SetY( button, 0 )
			}
		}
	}

	array<int> rowSizes
	foreach ( row in buttonRows )
		rowSizes.append( row.len() )

	int baseRowIndex = int( floor( (rowSizes.len() - 1) / 2.0 ) )
	array<var> baseRow = buttonRows[baseRowIndex]

	
	int baseRowScreenWidth = buttonWidth + ((baseRow.len() - 1) * spaceShift)
	int baseRowXPos = baseRowScreenWidth / 2

	foreach ( rIndex, row in buttonRows )
	{
		bool hasLeadingSpace = rIndex > baseRowIndex && row.len() < baseRow.len()

		rowData[rIndex].hasLeadingSpace = hasLeadingSpace
		rowData[rIndex].xPos = baseRowXPos + (rowShift * (rIndex - baseRowIndex)) + (hasLeadingSpace ? (spaceShift * -1) : 0 )

		foreach ( bIndex, button in row )
		{
			if ( bIndex == 0 )
			{
				Hud_SetPinSibling( button, "Anchor" )
				Hud_SetX( button, rowData[rIndex].xPos )
				Hud_SetY( button, rowData[rIndex].yPos )
			}
		}
	}

	foreach ( row in buttonRows )
		SetNav( row )


	SetNavUpDown(buttonRows)






















}


void function SetNavUpDown( array< array<var> > buttons )
{
	Assert( buttons.len() > 0 )

	foreach (rIndex, array<var> row in buttons )
	{
		bool isFirstRow = ( rIndex == 0 )
		bool isLastRow =  ( rIndex == buttons.len() - 1 )

		foreach (cIndex, var character in row )
		{
			array<var> previousRow = buttons[ isFirstRow ? (buttons.len() - 1) : (rIndex - 1) ]
			int previousCharacter = int(clamp(cIndex, 0, (previousRow.len()-1)))
			Hud_SetNavUp( character, previousRow[previousCharacter] )

			array<var> nextRow = buttons[ isLastRow ? 0 : (rIndex + 1) ]
			int nextCharacter = int(clamp(cIndex, 0, (nextRow.len()-1)))
			Hud_SetNavDown( character, nextRow[nextCharacter] )
		}
	}

}


void function SetNav( array<var> buttons )
{
	Assert( buttons.len() > 0 )

	var first = buttons[0]
	var last  = buttons[buttons.len() - 1]
	var prev
	var next
	var button

	for ( int i = 0; i < buttons.len(); i++ )
	{
		button = buttons[i]

		if ( button == first )
			prev = last
		else
			prev = buttons[i - 1]

		if ( button == last )
			next = first
		else
			next = buttons[i + 1]

		Hud_SetNavLeft( button, prev )
		Hud_SetNavRight( button, next )

		
		
	}
}




const array<string> LANGUAGES_WITHOUT_TRAILING_ZEROS_WHEN_TRUNCATED = [
	"german",
	"italian",
	"korean",
	"spanish",
	"schinese",
	"tchinese",
	"japanese",
]

string function LocalizeAndShortenNumber_Int( int number, int maxDisplayIntegral = 3, int maxDisplayDecimal = 0 )
{
	return LocalizeAndShortenNumber_Float( float(number), maxDisplayIntegral, maxDisplayDecimal )
}




string function LocalizeAndShortenNumber_Float( float number, int maxDisplayIntegral = 3, int maxDisplayDecimal = 0 )
{
	




#if SHORTEN_NUMBER_DBG
		printf( "ShortenNumberDebug: Shortening %f with max Integrals of %i and max decimals of %i\n", number, maxDisplayIntegral, maxDisplayDecimal )
#endif

	if ( number == 0.0 )
		return "0"

	string decimalSeparator     = Localize( "#DECIMAL_SEPARATOR" )

	float integral = floor( number )
	int digits = int( integral ) > 0 ? int( floor( log10( integral ) + 1 ) ) : 0

	






	if ( digits > maxDisplayIntegral )
	{
		Assert( maxDisplayIntegral >= 3, "LocalizeAndShortenNumber does not work with extremely small number limits where large numbers are possible" )

#if SHORTEN_NUMBER_DBG
			printf( "ShortenNumberDebug: Number too large for display - %i digits. Shortening to 3 digits\n", digits )
#endif

		int powToRaiseTo      = int( floor( ( digits - 1 ) / 3 ) * 3 ) 
		float truncNumber     = integral / pow( 10, powToRaiseTo )
		maxDisplayDecimal     = 3 - string( int( truncNumber ) ).len()
		string formatString   = "%0." + string( maxDisplayDecimal ) + "f" 
		string truncNumString = format( formatString, truncNumber ) 

		Assert( int( truncNumber ) >= 1 ) 

#if SHORTEN_NUMBER_DBG
			printf( "ShortenNumberDebug: Number shortened to %0.3f and then formatted using format string of '%s' to %s", truncNumber, formatString, truncNumString )
#endif

		string finalDisplayNumber = truncNumString

		
		int dotLocation = truncNumString.find( "." )
		if ( int( truncNumString ) == 1000 ) 
		{
			finalDisplayNumber = format( "1%s00", decimalSeparator )
			digits++ 
		}
		else if ( dotLocation != -1 )
		{
			Assert( dotLocation != truncNumString.len() - 1 )
			string intPart = truncNumString.slice( 0, dotLocation )
			string decPart = truncNumString.slice( dotLocation + 1 )
			if ( intPart.len() == 3 ) 
			{
				finalDisplayNumber = intPart
			}
			else
			{
				if ( intPart.len() == 2 && decPart.len() == 2 ) 
					decPart = decPart.slice( 0, 1 ) 

				finalDisplayNumber = intPart + decimalSeparator + decPart
			}
		}

		string integralSuffixLocKey = ""
		if ( digits >= 10 ) 
			integralSuffixLocKey = "#STATS_VALUE_BILLIONS"
		else if ( digits >= 7 )
			integralSuffixLocKey = "#STATS_VALUE_MILLIONS"
		else if ( digits >= 4 )
			integralSuffixLocKey = "#STATS_VALUE_THOUSANDS"
		else
			Assert( false, "unhandled metric suffix value" )

		return Localize( integralSuffixLocKey, finalDisplayNumber )
	}

	
	bool hideTrailingZeros = true 
	string formatString    = "%0." + string( maxDisplayDecimal ) + "f" 
	string fullNumString   = format( formatString, number ) 
	string integralString  = format( "%0.0f", integral )
	int dotLocation        = fullNumString.find( "." )
	string decimalString   = ""

	if ( dotLocation != -1 && hideTrailingZeros )
	{
		Assert( dotLocation != fullNumString.len() - 1 )
		decimalString = fullNumString.slice( dotLocation + 1 )

#if SHORTEN_NUMBER_DBG
			printf( "ShortenNumberDebug: decimal string is %s\n", decimalString )
#endif

		if ( int( decimalString ) == 0 )
		{
			decimalString = ""
		}
		else
		{
			float deciVal = float( decimalString )
			while ( deciVal % 10 == 0 ) 
				deciVal = deciVal / 10

			int numLeadingZeros = decimalString.len() - string( int( decimalString ) ).len()
			decimalString = ""
			for ( int i = 0; i < numLeadingZeros; i++ )
				decimalString += "0"
			decimalString += string( deciVal )
		}
	}

#if SHORTEN_NUMBER_DBG
		printf( "ShortenNumberDebug: Adding integral separators to %s\n", integralString )
#endif

	
	string thousandsSeparator      = Localize( "#THOUSANDS_SEPARATOR" )
	string separatedIntegralString = ""
	int integralsAdded             = 0
	for ( int i = integralString.len(); i > 0; i-- )
	{
		string num = integralString.slice( i - 1, i )
		if ( ( separatedIntegralString.len() - integralsAdded ) % 3 == 0 && separatedIntegralString.len() > 0 )
		{
			integralsAdded++
			separatedIntegralString = num + thousandsSeparator + separatedIntegralString
		}
		else
		{
			separatedIntegralString = num + separatedIntegralString
		}

#if SHORTEN_NUMBER_DBG
			printf( "ShortenNumberDebug: Separated Integral Progress: %s\n", separatedIntegralString )
#endif
	}

#if SHORTEN_NUMBER_DBG
		printf( "ShortenNumberDebug: Separated Integral String Complete: %s\n", separatedIntegralString )
#endif

	string finalDisplayNumber = separatedIntegralString
	if ( decimalString != "" )
		finalDisplayNumber = separatedIntegralString + decimalSeparator + decimalString

	return finalDisplayNumber
}


bool function IsTenThousandOrMore( var value )
{
	return value >= 10000
}

#if DEV
void function DEV_UnitTestShortenNumber()
{
	table< array< int >, string > intTestCases = {
		[[ 1000, 3, 3 ]] = "1.00k",
		[[ 1009, 3, 3 ]] = "1.01k",
		[[ 1999, 3, 3 ]] = "2.00k",
		[[ 9900, 3, 3 ]] = "9.90k",
		[[ 9990, 3, 3 ]] = "9.99k",
		[[ 9999, 3, 3 ]] = "10.0k",

		[[ 10000, 4, 2 ]] = "10.0k",
		[[ 10099, 4, 2 ]] = "10.1k",
		[[ 19999, 4, 2 ]] = "20.0k",
		[[ 99900, 4, 2 ]] = "99.9k",
		[[ 99990, 4, 2 ]] = "100k",
		[[ 99999, 4, 2 ]] = "100k",

		[[ 100000, 5, 1 ]] = "100k",
		[[ 100999, 5, 1 ]] = "101k",
		[[ 199999, 5, 1 ]] = "200k",
		[[ 999900, 5, 1 ]] = "1.00M",
		[[ 999990, 5, 1 ]] = "1.00M",
		[[ 999999, 5, 1 ]] = "1.00M",

		[[ 10000, 3, 3 ]] = "10.0k",
		[[ 10099, 3, 3 ]] = "10.1k",
		[[ 19999, 3, 3 ]] = "20.0k",
		[[ 99900, 3, 3 ]] = "99.9k",
		[[ 99990, 3, 3 ]] = "100k",
		[[ 99999, 3, 3 ]] = "100k",

		[[ 100000, 4, 2 ]] = "100k",
		[[ 100999, 4, 2 ]] = "101k",
		[[ 199999, 4, 2 ]] = "200k",
		[[ 999900, 4, 2 ]] = "1.00M",
		[[ 999990, 4, 2 ]] = "1.00M",
		[[ 999999, 4, 2 ]] = "1.00M",
	}

	table< array< float >, string > floatTestCases = {
		[[ 1.049, 3, 2 ]]			= "1.05",
		[[ 1000.5999, 5, 3 ]]       = "1,000.6",
		[[ 1000.5999, 5, 2 ]]       = "1,000.6",
		[[ 1000.999, 5, 3 ]]        = "1,000.999",
		[[ 1000000.999, 7, 2 ]]     = "1,000,001",
	}

	int num
	int maxInt
	int maxDec
	string outString
	foreach ( array< int > test, string expected in intTestCases )
	{
		num    = test[0]
		maxInt = test[1]
		maxDec = test[2]
		outString = LocalizeAndShortenNumber_Int( num, maxInt, maxDec )
		printt( format( "LocalizeAndShortenNumberDbg: number: %d | maxIntegral: %d | maxDecimal: %d --> %s", num, maxInt, maxDec, outString ) )
		Assert( expected == outString )
	}

	float flt
	foreach ( array< float > test, string expected in floatTestCases )
	{
		flt    = test[0]
		maxInt = int(floor(test[1]))
		maxDec = int(floor(test[2]))
		outString = LocalizeAndShortenNumber_Float( flt, maxInt, maxDec )
		printt( format( "LocalizeAndShortenNumberDbg: number: %0.3f | maxIntegral: %d | maxDecimal: %d --> %s", flt, maxInt, maxDec, outString ) )
		Assert( expected == outString )
	}
}
#endif
