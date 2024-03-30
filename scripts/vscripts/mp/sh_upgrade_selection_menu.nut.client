global function UpgradeSelectionMenu_Init


global function UpgradeSelectionMenu_IsActive







global function UpgradeSelectionMenu_Open
global function UpgradeSelectionMenu_HandleKeyInput
global function UpgradeSelectionMenu_MockSelection
global function UpgradeSelectionMenu_UpdateChoices
global function UpgradeSelectionMenu_TryClose


void function UpgradeSelectionMenu_Init()
{

		RegisterSignal( "UpgradeSelectionMenuClose" )
		AddOnSpectatorTargetChangedCallback( UpgradeSelectionMenu_OnSpectatorChanged )

}














bool function UpgradeSelectionMenu_IsActive()
{

	return file.upgradesSelectionMenu != null




}



struct {
	var upgradesSelectionMenu
}file

int s_currentChoice = -1
int s_latestValidChoice = -1
float s_latestValidChoiceTime = -1000.0
float s_latestViewInputResetTime = -1000.0

const string WHEEL_SOUND_ON_FOCUS = "menu_focus"
const string WHEEL_SOUND_ON_CLOSE = "menu_back"
const float MOCK_UPGRADE_SELECTION_WAIT_TIME = .5

void function ResetViewInput()
{
	s_currentChoice = -1
	s_latestValidChoice = -1
	s_latestValidChoiceTime = -1000.0
	s_latestViewInputResetTime = Time()

	ResetMouseInput()
}

vector s_mousePad
void function ResetMouseInput()
{
	s_mousePad = <0, 0, 0>
}
vector function ProcessMouseInput( float deltaX, float deltaY )
{
	float MAX_BOUNDS = 200.0

	s_mousePad = <s_mousePad.x + deltaX, s_mousePad.y + deltaY, 0.0>

	
	{
		float lenRaw = Length( s_mousePad )
		if ( lenRaw > MAX_BOUNDS )
			s_mousePad = (s_mousePad / lenRaw * MAX_BOUNDS)
	}

	float lenNow = Length( s_mousePad )
	if ( lenNow < 25.0 )
	{
		
		return <0, 0, 0>
	}

	vector result = (s_mousePad / Length( s_mousePad ))
	
	return result
}

void function UpgradeSelectionMenu_UpdateChoices(entity player)
{
	if (!file.upgradesSelectionMenu || !IsValid( player ) || player != GetLocalViewPlayer())
		return

	array<int> upgradeChoices = UpgradeCore_GetCurrentLevelChoices( player )
	array<string> numberStrArr = ["first", "second"]
	for( int i=0; i < upgradeChoices.len() && i < numberStrArr.len();i++ )
	{
		UpgradeCoreChoice choice =  UpgradeCore_GetUpgradeChoiceStructByIndex( player, upgradeChoices[i] )
		string numberStr = numberStrArr[i]
		RuiSetString( file.upgradesSelectionMenu, numberStr + "ChoiceTitle", choice.title )
		RuiSetString( file.upgradesSelectionMenu, numberStr + "ChoiceDesc", choice.shortDesc )
		RuiSetImage( file.upgradesSelectionMenu, numberStr + "ChoiceImage", choice.icon )
	}

	if( upgradeChoices.len() > 0 )
	{
		int tier = upgradeChoices[0] / 2 + 2
		RuiSetInt( file.upgradesSelectionMenu, "upgradeTier", tier )
	}

	UpgradeCore_UpdateMeterRuiVisibility( player )
}

void function UpgradeSelectionMenu_Open()
{
	if( UpgradeCore_AreUpgradesLocked() )
		return

	if( UpgradeSelectionMenu_IsActive() )
		return

	if( !UpgradeCore_ShowHudUpgradeSelection() )
		return

	Announcements_SetVisible(false)

	RunUIScript( "ClientToUi_SetUpgradeSelectionOpen", true )

	file.upgradesSelectionMenu = CreateCockpitPostFXRui( $"ui/upgrade_core_selection_menu.rpak", RUI_SORT_SCREENFADE + 2 )

	ResetViewInput()

	HudInputContext inputContext
	inputContext.keyInputCallback = UpgradeSelectionMenu_HandleKeyInput
	inputContext.viewInputCallback = UpgradeSelectionMenu_HandleViewInput
	inputContext.hudInputFlags = (HIF_BLOCK_WAYPOINT_FOCUS)
	HudInput_PushContext( inputContext )

	entity player = GetLocalViewPlayer()
	thread UpgradeSelectionMenu_ListenForDamageThread( player )
	UpgradeSelectionMenu_UpdateChoices( player )
}

void function PlayerWaittillHealthChanged( entity player )
{
	player.WaitSignal( "HealthChanged", "ShieldChanged" )
}

void function UpgradeSelectionMenu_ListenForDamageThread( entity player )
{
	player.EndSignal( "OnDestroy" )
	player.EndSignal( "OnDeath" )
	player.EndSignal( "UpgradeSelectionMenuClose" )
	player.EndSignal( "FreefallStarted" )

	entity drone = CryptoDrone_GetPlayerDrone( player )
	if ( IsValid( drone ) )
		drone.EndSignal( "CameraViewStart" )

	float lastHealthFrac = GetHealthFrac( player )
	float lastShieldFrac = GetShieldHealthFrac( player )

	OnThreadEnd(
		function() : ()
		{
			thread UpgradeSelectionMenu_Shutdown()
		}
	)

	while( true )
	{
		waitthread PlayerWaittillHealthChanged( player )
		if( file.upgradesSelectionMenu == null )
			break

		float healthFrac = GetHealthFrac( player )
		if ( healthFrac < lastHealthFrac )
		{
			break
		}

		float shieldFrac = GetShieldHealthFrac( player )
		if ( shieldFrac < lastShieldFrac )
		{
			break
		}

		lastHealthFrac = healthFrac
		lastShieldFrac = shieldFrac
	}
}

float s_pageSwitchTime = 0.0
const float BUTTON_PAIR_ACTIVITION_TIME = 0.25
bool function UpgradeSelectionMenu_HandleKeyInput( int key )
{
	if ( GetLocalClientPlayer() != GetLocalViewPlayer() )
		return false

	bool shouldExecute    = false
	bool shouldCancelMenu = false
	bool shouldConsumeInput = false
	int choice            = -1

	{
		switch ( key )
		{
			case KEY_1:
				choice = 0
				break

			case KEY_2:
				choice = 1
				break

			case KEY_3:
				choice = 2
				break

			case KEY_4:
				choice = 3
				break

			case KEY_5:
				choice = 4
				break

			case KEY_6:
				choice = 5
				break

			case KEY_7:
				choice = 6
				break

			case KEY_8:
				choice = 7
				break

			case BUTTON_A:
			case MOUSE_LEFT:
				shouldExecute = true
				shouldConsumeInput = true
				break
			case BUTTON_B:
			case KEY_ESCAPE:
			case MOUSE_RIGHT:
				shouldCancelMenu = true
				shouldConsumeInput = true
				break
		}
	}

	if ( ButtonIsBoundToPing( key ) )
	{
		shouldCancelMenu = true
		shouldConsumeInput = true
	}
	else if( ButtonIsBoundToAction( key, "+melee" ) )
	{
		shouldCancelMenu = true
	}
	else if( ButtonIsBoundToAction( key, "+weaponcycle" ) )
	{
		shouldCancelMenu = true
	}
	else if( ButtonIsBoundToAction( key, "+offhand1" ) )
	{
		shouldCancelMenu = true
	}
	else if( ButtonIsBoundToAction( key, "+offhand4" ) )
	{
		shouldCancelMenu = true
	}
	else if( ButtonIsBoundToAction( key, "+use" ) )
	{
		shouldCancelMenu = true
	}

	if ( shouldExecute )
	{
		if ( s_currentChoice >= 0 )
		{
			UpgradeSelectionMenu_ApplyUpgrade( s_currentChoice )
		}

		shouldCancelMenu = true
	}

	if ( shouldCancelMenu )
	{
		UpgradeSelectionMenu_Shutdown()
		entity player = GetLocalViewPlayer()
		if ( IsValid( player ) )
			EmitSoundOnEntity( player, WHEEL_SOUND_ON_CLOSE )

	}

	return shouldConsumeInput
}

void function UpgradeSelectionMenu_MockSelection( int upgradeIndex )
{
	entity player = GetLocalViewPlayer()
	if( !IsValid( player ) )
		return

	UpgradeSelectionMenu_Open()
	player.EndSignal( "UpgradeSelectionMenuClose" )
	int choice = upgradeIndex % 2
	wait( MOCK_UPGRADE_SELECTION_WAIT_TIME )
	SetCurrentChoice( choice )
	EmitSoundOnEntity( player, WHEEL_SOUND_ON_FOCUS )
	wait( MOCK_UPGRADE_SELECTION_WAIT_TIME )
	EmitSoundOnEntity( player, UPGRADE_SELECTED_SOUND )
	thread 	UpgradeSelectionMenu_Shutdown()
}

void function UpgradeSelectionMenu_OnSpectatorChanged( entity spectatingPlayer, entity oldSpectatorTarget, entity newSpectatorTarget )
{
	if( UpgradeSelectionMenu_IsActive() )
	{
		thread UpgradeSelectionMenu_Shutdown()
	}
}

void function UpgradeSelectionMenu_ApplyUpgrade( int choice )
{
	entity player = GetLocalViewPlayer()
	array<int> upgradeChoices = UpgradeCore_GetCurrentLevelChoices( player )
	if( choice < upgradeChoices.len() )
		UpgradeCore_SelectOption( upgradeChoices[choice], true )
}

void function UpgradeSelectionMenu_TryClose()
{
	if( UpgradeSelectionMenu_IsActive() )
		UpgradeSelectionMenu_Shutdown()
}

void function UpgradeSelectionMenu_Shutdown()
{
	Announcements_SetVisible(true)
	entity player = GetLocalViewPlayer()
	if( player )
	{
		player.Signal( "UpgradeSelectionMenuClose" )
	}
	RunUIScript( "ClientToUi_SetUpgradeSelectionOpen", false )
	if( file.upgradesSelectionMenu != null )
	{
		RuiDestroyIfAlive( file.upgradesSelectionMenu )
		file.upgradesSelectionMenu = null
		UpgradeCore_UpdateMeterRuiVisibility( player )
		HudInput_PopContext()
	}
}

bool function UpgradeSelectionMenu_HandleViewInput( float x, float y )
{
	if ( GetLocalClientPlayer() != GetLocalViewPlayer() )
		return false

	
	{
		float lockoutTime            = IsControllerModeActive() ? 0.0 : 0.01
		float deltaSinceInputStarted = (Time() - s_latestViewInputResetTime)
		if ( deltaSinceInputStarted < lockoutTime )
			return false
	}

	int optionCount = 2
	int choice      = -1

	
	float lenCutoff = IsControllerModeActive() ? ((s_currentChoice < 0) ? 0.8 : 0.4) : ((s_currentChoice < 0) ? 0.8 : 0.4)

	RuiSetFloat2( file.upgradesSelectionMenu, "inputVec", <0, 0, 0> )
	vector inputVec = IsControllerModeActive() ? <x, y, 0.0> : ProcessMouseInput( x, y )
	float inputLen  = Length( inputVec )
	if ( optionCount <= 0 )
	{
		choice = -1
	}
	else if ( inputLen > lenCutoff )
	{
		float circle = 2.0 * PI
		float angle  = atan2( inputVec.x, inputVec.y )        
		if ( angle < 0.0 )
			angle += circle

		float slotWidth = (circle / float( optionCount ))
		angle += slotWidth * 0.5

		choice = (int( ( ( angle + PI / 2 ) / circle) * optionCount ) % optionCount)

		

		vector ruiInputVec = IsControllerModeActive() ? Normalize( inputVec ) : inputVec
		RuiSetFloat2( file.upgradesSelectionMenu, "inputVec", Normalize( inputVec ) )
	}
	else
	{
		if ( IsControllerModeActive() )
			choice = s_currentChoice 
		else
			choice = s_currentChoice
	}

	if ( (choice >= 0) && (choice != s_currentChoice) )
	{
		entity player = GetLocalViewPlayer()
		EmitSoundOnEntity( player, WHEEL_SOUND_ON_FOCUS )
	}

	SetCurrentChoice( choice )
	return true
}

void function SetCurrentChoice( int choice )
{
	s_currentChoice = choice
	RuiSetInt( file.upgradesSelectionMenu, "highlightedChoice", choice )
}

