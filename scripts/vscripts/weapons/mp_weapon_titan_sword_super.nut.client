
global function MpWeaponTitanSword_Super_Init
global function TitanSword_Super_OnWeaponActivate
global function TitanSword_Super_OnWeaponDeactivate
global function TitanSword_Super_ClearMods


global function ServerToClient_TitanSword_StartSuperFx
global function ServerToClient_TitanSword_StopSuperFx
global function ServerToClient_TitanSword_SuperReady
global function ServerToClient_TitanSword_AddChargeFx







#if DEV




#endif

global function TitanSword_Super_IsActive



const string TITAN_SWORD_SUPER_MOD = "super"


const string PVAR_TITAN_SWORD_MAX_SUPER = "titan_sword_super_max"
const string PVAR_TITAN_SWORD_SUPER_WHIFF_RESET = "titan_sword_super_whiff_reset"
const string PVAR_TITAN_SWORD_SUPER_TICK = "titan_sword_super_tick"
const string PVAR_TITAN_SWORD_SUPER_PER_TICK = "titan_sword_super_per_tick"
const string PVAR_TITAN_SWORD_SUPER_DECAY = "titan_sword_super_decay"


const string SIG_TITAN_SWORD_DESTROY_SUPER_RUI = "TitanSword_DestroySuperRui"
const string SIG_TITAN_SWORD_BEEP_THREAD = "TitanSword_BeepThread"
const string SIG_TITAN_SWORD_SUPER_STOP = "TitanSword_Super_Stop"

const string NETVAR_TITAN_SWORD_SUPER = "TitanSwordSuper"


const array<int> TITAN_SWORD_SLOTS_TO_LOCK = [
	WEAPON_INVENTORY_SLOT_PRIMARY_0,
	WEAPON_INVENTORY_SLOT_PRIMARY_1,
	WEAPON_INVENTORY_SLOT_PRIMARY_2,
	WEAPON_INVENTORY_SLOT_PRIMARY_3,
	WEAPON_INVENTORY_SLOT_PRIMARY_4,
]

const float TITAN_SWORD_SUPER_ACTIVATION_DURATION = 2.33


const asset VFX_TITAN_SWORD_SUPER_ACTIVATE = $"P_pilot_powerup_flash"
const asset VFX_TITAN_SWORD_SUPER_1P = $"P_pilot_powerup_FP"
const asset VFX_TITAN_SWORD_SUPER_3P = $"P_pilot_powerup_chest"
const asset VFX_TITAN_SWORD_SUPER_WEAPON_START_GLOWING_1P = $"P_pilot_sword_charging_FP" 
const asset VFX_TITAN_SWORD_SUPER_WEAPON_START_GLOWING_3P = $"P_pilot_sword_charging_3P" 
const asset VFX_TITAN_SWORD_SUPER_WEAPON_ACTIVATE_1P = $"P_pilot_sword_charged_FP" 
const asset VFX_TITAN_SWORD_SUPER_WEAPON_ACTIVATE_3P = $"P_pilot_sword_charged_3P"
const asset VFX_TITAN_SWORD_SUPER_WEAPON_CONTINUOUS_1P = $"P_pilot_sword_swipe_super_FP" 
const asset VFX_TITAN_SWORD_SUPER_WEAPON_CONTINUOUS_3P = $"P_pilot_sword_swipe_super_3P"


const string SFX_TITAN_SWORD_SUPER_READY = "titansword_special_super_ready_1p"
const string SFX_TITAN_SWORD_SUPER_START_1P = "titansword_special_super_start_1p"
const string SFX_TITAN_SWORD_SUPER_START_3P = "titansword_special_super_start_3p"
const string SFX_TITAN_SWORD_SUPER_ACTIVATE_1P = "titansword_special_super_activate_1p"
const string SFX_TITAN_SWORD_SUPER_ACTIVATE_3P = "titansword_special_super_activate_3p"


const asset RUI_TITAN_SWORD_SUPER_HUD = $"ui/weapon_hud_charged_gh.rpak"

struct
{

		int superFxHandle

	var superRui
}file


void function MpWeaponTitanSword_Super_Init()
{
	PrecacheParticleSystem( VFX_TITAN_SWORD_SUPER_ACTIVATE )
	PrecacheParticleSystem( VFX_TITAN_SWORD_SUPER_1P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_SUPER_3P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_SUPER_WEAPON_START_GLOWING_1P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_SUPER_WEAPON_START_GLOWING_3P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_SUPER_WEAPON_ACTIVATE_1P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_SUPER_WEAPON_ACTIVATE_3P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_SUPER_WEAPON_CONTINUOUS_1P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_SUPER_WEAPON_CONTINUOUS_3P )

	RegisterNetworkedVariable( NETVAR_TITAN_SWORD_SUPER, SNDC_PLAYER_EXCLUSIVE, SNVT_TIME, -2.0 )
	Remote_RegisterServerFunction( "ClientCallback_TitanSword_SelectFirePressed" )
	Remote_RegisterClientFunction( "ServerToClient_TitanSword_SuperReady", "entity" )
	Remote_RegisterClientFunction( "ServerToClient_TitanSword_StartSuperFx" )
	Remote_RegisterClientFunction( "ServerToClient_TitanSword_StopSuperFx" )
	Remote_RegisterClientFunction( "ServerToClient_TitanSword_AddChargeFx" )


		RegisterSignal( SIG_TITAN_SWORD_DESTROY_SUPER_RUI )

		RegisterConCommandTriggeredCallback( "+scriptCommand3", OnSelectFirePressed )

		AddCallback_OnPrimaryWeaponStatusUpdate( OnPrimaryWeaponStatusUpdate_TitanSwordSuper )

		
		
		







}

void function TitanSword_Super_StartGlowingVFX( entity weapon )
{
	weapon.PlayWeaponEffect( VFX_TITAN_SWORD_SUPER_WEAPON_START_GLOWING_1P, VFX_TITAN_SWORD_SUPER_WEAPON_START_GLOWING_3P, "blade_mid" )
}

void function TitanSword_Super_StopGlowingVFX( entity weapon )
{
	weapon.StopWeaponEffect( VFX_TITAN_SWORD_SUPER_WEAPON_START_GLOWING_1P, VFX_TITAN_SWORD_SUPER_WEAPON_START_GLOWING_3P )
}

void function TitanSword_Super_StartActivationVFX( entity weapon )
{
	weapon.PlayWeaponEffect( VFX_TITAN_SWORD_SUPER_WEAPON_ACTIVATE_1P, VFX_TITAN_SWORD_SUPER_WEAPON_ACTIVATE_3P, "blade_mid" )
}

void function TitanSword_Super_StopActivationVFX( entity weapon )
{
	weapon.StopWeaponEffect( VFX_TITAN_SWORD_SUPER_WEAPON_ACTIVATE_1P, VFX_TITAN_SWORD_SUPER_WEAPON_ACTIVATE_3P )
}


void function TitanSword_Super_StartContinuousVFX( entity weapon )
{
	
	weapon.PlayWeaponEffect( VFX_TITAN_SWORD_SUPER_WEAPON_CONTINUOUS_1P, VFX_TITAN_SWORD_SUPER_WEAPON_CONTINUOUS_3P, "blade_mid", true )
}

void function TitanSword_Super_StopContinuousVFX( entity weapon )
{
	weapon.StopWeaponEffect( VFX_TITAN_SWORD_SUPER_WEAPON_CONTINUOUS_1P, VFX_TITAN_SWORD_SUPER_WEAPON_CONTINUOUS_3P )
}

void function TitanSword_Super_OnWeaponActivate( entity player, entity weapon )
{

		thread TitanSword_SuperRui_Thread( weapon, player )



		if ( !InPrediction() )
			return


	if ( TitanSword_Super_IsActive( player ) )
	{
		TitanSword_Super_StartContinuousVFX( weapon )
		if ( !weapon.HasMod( TITAN_SWORD_SUPER_MOD ) )
		{
			weapon.AddMod( TITAN_SWORD_SUPER_MOD )
		}
	}
	else
	{
		if ( !weapon.HasMod( TITAN_SWORD_SUPER_MOD ) )
		{
			weapon.RemoveMod( TITAN_SWORD_SUPER_MOD )
		}
	}




}
void function TitanSword_Super_OnWeaponDeactivate( entity player, entity weapon )
{
	TitanSword_Super_StopGlowingVFX( weapon )
	TitanSword_Super_StopActivationVFX( weapon )
	TitanSword_Super_StopContinuousVFX( weapon )
}

void function TitanSword_Super_ClearMods( entity weapon )
{
}

float function TitanSword_Super_GetCharge( entity player )
{
	return player.GetPlayerNetTime( NETVAR_TITAN_SWORD_SUPER )
}

bool function TitanSword_Super_HasCharge( entity player )
{
	return Time() >= TitanSword_Super_GetCharge( player )
}

bool function TitanSword_Super_ChargingStopped( entity player )
{
	return player.GetPlayerNetTime( NETVAR_TITAN_SWORD_SUPER ) <= -1.0
}

bool function TitanSword_Super_IsFirstPickup( entity player )
{
	return player.GetPlayerNetTime( NETVAR_TITAN_SWORD_SUPER ) <= -2.0
}































































































































float function TitanSword_Super_GetChargeScale( entity player )
{
	if ( TitanSword_Super_IsActive( player ) )
	{
		float active = StatusEffect_GetTimeRemaining( player, eStatusEffect.titan_sword_super )
		float diff   = active / GetWeaponInfoFileKeyField_GlobalFloat( TITAN_SWORD_WEAPON_REF, "super_active_sec" )
		return clamp( diff, 0.0, 1.0 )
	}

	if ( TitanSword_Super_ChargingStopped( player ) )
		return 0.0

	if ( TitanSword_Super_HasCharge( player ) )
		return 1.0

	float charge = TitanSword_Super_GetCharge( player ) - Time()
	
	float diff   = charge / GetWeaponInfoFileKeyField_GlobalFloat( TITAN_SWORD_WEAPON_REF, "super_cooldown_sec" )

	return clamp( 1.0 - diff, 0.0, 1.0 )
}

bool function TitanSword_Super_IsActive( entity player )
{
	return StatusEffect_HasSeverity( player, eStatusEffect.titan_sword_super )
}








































































































































































void function OnSelectFirePressed( entity player )
{
	if ( player != GetLocalViewPlayer() )
		return

	if ( !TryActivateSuper( player ) )
		return

	Remote_ServerCallFunction( "ClientCallback_TitanSword_SelectFirePressed" )

	const int randomHints = 1
	int hintSelection = RandomIntRange( 0, randomHints )

	entity activeWeapon = TitanSword_GetMainWeapon( player )
	if ( IsValid( activeWeapon ) )
		TitanSword_Super_StartGlowingVFX( activeWeapon )

	string hintBase = "#WPN_TITAN_SWORD_SUPER_ACTIVATED_BASE"
	string hintStr  = "#WPN_TITAN_SWORD_SUPER_ACTIVATED_" + hintSelection
	
	AnnouncementMessageRight( player, Localize( hintBase ) + Localize( hintStr ), "", <1, 1, 0>, $"", 5.0 )
}

void function ServerToClient_TitanSword_StartSuperFx()
{
	entity player = GetLocalViewPlayer()

	entity cockpit = player.GetCockpit()
	if ( !IsValid( cockpit ) )
		return

	Assert( !EffectDoesExist( file.superFxHandle ), "tried to start a second screen fx" )

	int fxID = GetParticleSystemIndex( VFX_TITAN_SWORD_SUPER_1P )
	file.superFxHandle = StartParticleEffectOnEntity( cockpit, fxID, FX_PATTACH_ABSORIGIN_FOLLOW, ATTACHMENTID_INVALID )
	EffectSetIsWithCockpit( file.superFxHandle, true )
	entity weapon = TitanSword_GetMainWeapon ( player )
	if ( IsValid( weapon ) )
	{
		TitanSword_Super_StopGlowingVFX( weapon )
		TitanSword_Super_StartActivationVFX( weapon )
		TitanSword_Super_StartContinuousVFX( weapon )
	}
	
}


void function ServerToClient_TitanSword_StopSuperFx()
{
	if ( !EffectDoesExist( file.superFxHandle ) )
		return

	EffectStop( file.superFxHandle, false, true )

	entity player = GetLocalViewPlayer()

	entity weapon = TitanSword_GetMainWeapon ( player )
	if ( IsValid( weapon ) )
	{
		TitanSword_Super_StopGlowingVFX( weapon )
		TitanSword_Super_StopActivationVFX( weapon )
		TitanSword_Super_StopContinuousVFX( weapon )
	}
	
}

void function ServerToClient_TitanSword_SuperReady( entity player )
{
	printt( "SUPER READY: " + player + " :: " + GetLocalViewPlayer() )
	if ( player != GetLocalViewPlayer() )
		return

	AddOnscreenPromptFunction( "quickchat", CreateQuickchatFunction( eCommsAction.QUICKCHAT_TITAN_SWORD_READY_FULL, player ), 8.0, Localize( "#WPN_TITAN_SWORD_SUPER_READY_COMMS" ) )
	
}
void function ServerToClient_TitanSword_AddChargeFx()
{
	if ( file.superRui != null )
		RuiSetGameTime( file.superRui, "flashStartTime", Time() )
}


bool function TryActivateSuper( entity player )
{
	if ( TitanSword_Super_IsActive( player ) )
		return false

	if ( !TitanSword_Super_HasCharge( player ) )
		return false

	if ( player.IsMantling() || player.IsWallRunning() || player.IsWallHanging() )
		return false

	entity activeWeapon = TitanSword_GetMainWeapon( player )
	if ( !IsValid( activeWeapon ) )
		return false

	if ( !activeWeapon.IsReadyToFire() )
		return false

	if ( activeWeapon.HasMod( TITAN_SWORD_SUPER_MOD ) )
		return false

	return true
}



void function OnPrimaryWeaponStatusUpdate_TitanSwordSuper( entity selectedWeapon, var weaponRui )
{
	if ( !IsValid( selectedWeapon ) )
		return

	
	entity activeWeapon         = GetLocalViewPlayer().GetActiveWeapon( eActiveInventorySlot.mainHand )
	bool switchToMeleeOrGrenade = IsBitFlagSet( selectedWeapon.GetWeaponTypeFlags(), (WPT_VIEWHANDS | WPT_GRENADE) )
	if ( IsValid( activeWeapon ) && activeWeapon != selectedWeapon )
	{
		if ( !(TitanSword_WeaponIsTitanSword( activeWeapon ) && switchToMeleeOrGrenade) )
			activeWeapon.Signal( SIG_TITAN_SWORD_DESTROY_SUPER_RUI )
	}

	if ( TitanSword_WeaponIsTitanSword( selectedWeapon ) )
	{
		entity player = selectedWeapon.GetWeaponOwner()
		thread TitanSword_SuperRui_Thread( selectedWeapon, player )
	}
}

void function TitanSword_SuperRui_Thread( entity weapon, entity player )
{
	AssertIsNewThread()

	if ( !IsValid( player ) )
		return

	if ( !IsLocalViewPlayer( player ) )
		return

	
	if ( weapon != player.GetActiveWeapon( eActiveInventorySlot.mainHand ) )
		return

	weapon.Signal( SIG_TITAN_SWORD_DESTROY_SUPER_RUI )
	weapon.EndSignal( "OnDestroy" )
	weapon.EndSignal( SIG_TITAN_SWORD_DESTROY_SUPER_RUI )
	weapon.EndSignal( SIG_TITAN_SWORD_DEACTIVATE )

	player.Signal( WEAPON_CHARGED_RUI_ABORT_SIGNAL )
	player.EndSignal( WEAPON_CHARGED_RUI_ABORT_SIGNAL )
	player.EndSignal( "OnDeath" )
	player.EndSignal( "OnDestroy" )

	string weaponName = weapon.GetWeaponClassName()
	var rui           = ClWeaponStatus_GetWeaponHudRui( player )

	if ( rui == null )
		return

	RuiDestroyNestedIfAlive( rui, "chargedHandle" )

	var nestedRui = RuiCreateNested( rui, "chargedHandle", RUI_TITAN_SWORD_SUPER_HUD )
	if ( nestedRui == null )
		return

	file.superRui = nestedRui
	OnThreadEnd(
		function() : ( player, weapon, rui, nestedRui )
		{
			RuiSetBool( rui, "showChargeBar", false )
			RuiSetBool( rui, "showChargeBarBorderColor", false )
			RuiDestroyNestedIfAlive( rui, "chargedHandle" )
			file.superRui = null
		}
	)

	vector frameColorBase    = SrgbToLinear( GetAmmoColorByType( "supply_drop" ) ) 
	vector frameColorCharged = SrgbToLinear( <247, 226, 70> / 255.0 )

	RuiSetFloat3( nestedRui, "frameColor", frameColorBase )
	RuiSetString( nestedRui, "chargeBarText", "" )
	RuiSetBool( nestedRui, "showChargeBar", true )
	RuiSetBool( rui, "showChargeBar", true )
	RuiSetBool( rui, "hasNoAmmo", true )
	RuiSetFloat3( rui, "chargeBarBorderOverlayColor", frameColorCharged )

	while( IsValid ( weapon ) && IsValid( player ) && file.superRui != null )
	{
		float superFrac       = TitanSword_Super_GetChargeScale( player )
		string chargeBarText  = ""
		bool showChargedState = true
		bool superIsActive    = TitanSword_Super_IsActive( player )

		if ( !superIsActive )
		{
			if ( superFrac >= 1.0 )
			{
				chargeBarText    = Localize( "#WPN_TITAN_SWORD_SUPER_READY" )
				showChargedState = true
			}
			else
			{
				chargeBarText = Localize( "#WPN_TITAN_SWORD_SUPER_CHARGING" )
				int chargePercent = int( floor( superFrac * 100 ) )
				chargeBarText += (" " + chargePercent + "%")
				showChargedState = false
			}
		}

		
		RuiSetString( nestedRui, "chargeBarText", chargeBarText )
		RuiSetFloat( nestedRui, "chargeBarFrac", superFrac )
		RuiSetFloat3( nestedRui, "chargeBGcolor", GetSuperColor( superFrac ) )
		RuiSetFloat3( nestedRui, "frameColor", showChargedState ? frameColorCharged : frameColorBase )
		RuiSetBool( nestedRui, "superActive", superIsActive )
		RuiSetBool( rui, "showChargeBarBorderColor", showChargedState )

		WaitFrame()
	}
}


vector function GetSuperColor( float frac )
{
	if ( frac < 1.0 )
		return <255, 234, 0>

	return <255, 0, 255>
}



#if DEV


























































#endif


