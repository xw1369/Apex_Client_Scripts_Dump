
global function MpWeaponHorizonHeirloomPrimary_Init
global function OnWeaponActivate_weapon_horizon_heirloom_primary
global function OnWeaponDeactivate_weapon_horizon_heirloom_primary

const string HORIZON_WEAPON = "mp_weapon_horizon_heirloom_primary"

const HORIZON_WEAPON_IDLE_FX_1P = $"P_wpn_morning_star_idle"
const HORIZON_WEAPON_IDLE_FX_3P = $"P_wpn_morning_star_idle_3p"
const HORIZON_WEAPON_BASE_FX_1P = $"P_wpn_morning_star_base"
const HORIZON_WEAPON_BASE_FX_3P = $"P_wpn_morning_star_base_3p"
const HORIZON_WEAPON_LIGHT_FX_1P = $"P_wpn_morning_star_light"
const HORIZON_WEAPON_LIGHT_FX_3P = $"P_wpn_morning_star_light_3p"
const HORIZON_WEAPON_HOLSTER_FX_1P = $"P_wpn_morning_star_holster"
const HORIZON_WEAPON_HOLSTER_FX_3P = $"P_wpn_morning_star_holster_3p"
const HORIZON_WEAPON_DRAWSPRINT_TWIST_FX_1P = $"P_wpn_morning_star_start_short"
const HORIZON_WEAPON_DRAWSPRINT_TWIST_FX_3P = $"P_wpn_morning_star_start_short_3p"
const HORIZON_WEAPON_DRAW_FIRST_FX_1P = $"P_wpn_morning_star_start_first_DELAY"
const HORIZON_WEAPON_DRAW_FIRST_FX_3P = $"P_wpn_morning_star_start_first_DELAY_3p"

void function MpWeaponHorizonHeirloomPrimary_Init()
{
	PrecacheParticleSystem( HORIZON_WEAPON_IDLE_FX_1P )
	PrecacheParticleSystem( HORIZON_WEAPON_IDLE_FX_3P )
	PrecacheParticleSystem( HORIZON_WEAPON_BASE_FX_1P )
	PrecacheParticleSystem( HORIZON_WEAPON_BASE_FX_3P )
	PrecacheParticleSystem( HORIZON_WEAPON_LIGHT_FX_1P )
	PrecacheParticleSystem( HORIZON_WEAPON_LIGHT_FX_3P )
	PrecacheParticleSystem( HORIZON_WEAPON_HOLSTER_FX_1P )
	PrecacheParticleSystem( HORIZON_WEAPON_HOLSTER_FX_3P )
	PrecacheParticleSystem( HORIZON_WEAPON_DRAWSPRINT_TWIST_FX_1P )
	PrecacheParticleSystem( HORIZON_WEAPON_DRAWSPRINT_TWIST_FX_3P )
	PrecacheParticleSystem( HORIZON_WEAPON_DRAW_FIRST_FX_1P )
	PrecacheParticleSystem( HORIZON_WEAPON_DRAW_FIRST_FX_3P )
	PrecacheImpactEffectTable( "melee_morning_star" )
	PrecacheImpactEffectTable( "melee_repulsor" )
	PrecacheImpactEffectTable( "melee_pen" )


		AddOnSpectatorTargetChangedCallback( OnSpectatorTargetChanged_Callback )


	RegisterScriptAnimWindowCallbacks( "Horizon_Heirloom", HorizonHeirloom_ScriptAnimWindowStartCallback, HorizonHeirloom_ScriptAnimWindowStopCallback )
}


void function OnSpectatorTargetChanged_Callback( entity player, entity prevTarget, entity newTarget )
{
	if( !IsValid( newTarget ) )
		return

	entity weapon = newTarget.GetActiveWeapon( eActiveInventorySlot.mainHand  )
	if( IsValid( weapon ) && weapon.GetWeaponClassName() == "mp_weapon_horizon_heirloom_primary" )
		StartPermanentVFX( weapon )
}


void function OnWeaponActivate_weapon_horizon_heirloom_primary( entity weapon )
{
	StartPermanentVFX( weapon )
}

void function OnWeaponDeactivate_weapon_horizon_heirloom_primary( entity weapon )
{








	StopPermanentVFX( weapon )
}

void function HorizonHeirloom_ScriptAnimWindowStartCallback( entity ent, string parameter )
{
	entity weapon

	if ( ent.IsWeaponX() )
		weapon = ent
	else
		weapon = ViewModel_GetWeapon( ent )

	
	if ( !IsValid( weapon ) || weapon.GetWeaponClassName() != HORIZON_WEAPON )
		return

	if( parameter == "Inspect_Orbit" || parameter == "Inspect_Beloved"  || parameter == "Inspect_Battery" )
	{
		StopPermanentVFX1P( weapon )
	}
	else if( parameter == "Draw_Button" || parameter == "Draw_Eureka" )
	{
		StopPermanentVFX1P( weapon )
	}
	else if( parameter == "First_Draw" )
	{
		StopPermanentVFX1P( weapon )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, HORIZON_WEAPON_LIGHT_FX_3P, "FX_LIGHT", true )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_DRAW_FIRST_FX_1P, HORIZON_WEAPON_DRAW_FIRST_FX_3P, "FX_CORE", true )
	}
	else if ( parameter == "Inspect_Pen" )
	{
		StopPermanentVFX1P( weapon )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_BASE_FX_1P, $"", "FX_BASE", true )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, $"", "FX_LIGHT", true )
	}
	else if ( parameter == "Holster" )
	{
		weapon.PlayWeaponEffect( HORIZON_WEAPON_IDLE_FX_1P, HORIZON_WEAPON_IDLE_FX_3P, "FX_CORE", true )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_BASE_FX_1P, HORIZON_WEAPON_BASE_FX_3P, "FX_BASE", true )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, HORIZON_WEAPON_LIGHT_FX_3P, "FX_LIGHT", true )
	}
	else if ( parameter == "Holster_Off" )
	{
		weapon.PlayWeaponEffect( HORIZON_WEAPON_HOLSTER_FX_1P, HORIZON_WEAPON_HOLSTER_FX_3P, "FX_CORE", true )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_BASE_FX_1P, HORIZON_WEAPON_BASE_FX_3P, "FX_BASE", true )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, HORIZON_WEAPON_LIGHT_FX_3P, "FX_LIGHT", true )
	}
	else if ( parameter == "Drawsprint_Twist" )
	{
		StopPermanentVFX1P( weapon )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_DRAWSPRINT_TWIST_FX_1P, HORIZON_WEAPON_DRAWSPRINT_TWIST_FX_3P, "FX_CORE", true )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_BASE_FX_1P, HORIZON_WEAPON_BASE_FX_3P, "FX_BASE", true )
		weapon.PlayWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, HORIZON_WEAPON_LIGHT_FX_3P, "FX_LIGHT", true )
	}
}

void function HorizonHeirloom_ScriptAnimWindowStopCallback( entity ent, string parameter )
{
	entity weapon

	if ( ent.IsWeaponX() )
		weapon = ent
	else
		weapon = ViewModel_GetWeapon( ent )

	
	if ( !IsValid( weapon ) || weapon.GetWeaponClassName() != HORIZON_WEAPON )
		return

	if( parameter == "Inspect_Orbit" || parameter == "Inspect_Beloved"  || parameter == "Inspect_Battery" )
	{
		StartPermanentVFX1P( weapon )
	}
	else if( parameter == "First_Draw" )
	{
		weapon.StopWeaponEffect( HORIZON_WEAPON_DRAW_FIRST_FX_1P, HORIZON_WEAPON_DRAW_FIRST_FX_3P )
		StartPermanentVFX1P( weapon )
	}
	else if ( parameter == "Inspect_Pen" )
	{
		weapon.StopWeaponEffect( HORIZON_WEAPON_BASE_FX_1P, $"" )
		weapon.StopWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, $"" )
		StartPermanentVFX1P( weapon )
	}
	else if ( parameter == "Drawsprint_Twist" )
	{
		weapon.StopWeaponEffect( HORIZON_WEAPON_DRAWSPRINT_TWIST_FX_1P, HORIZON_WEAPON_DRAWSPRINT_TWIST_FX_3P )
		weapon.StopWeaponEffect( HORIZON_WEAPON_BASE_FX_1P, HORIZON_WEAPON_BASE_FX_3P )
		weapon.StopWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, HORIZON_WEAPON_LIGHT_FX_3P )
	}

}

void function StartPermanentVFX( entity weapon )
{
	StopPermanentVFX( weapon ) 

	weapon.PlayWeaponEffect( HORIZON_WEAPON_BASE_FX_1P, HORIZON_WEAPON_BASE_FX_3P, "FX_BASE", true )
	weapon.PlayWeaponEffect( HORIZON_WEAPON_IDLE_FX_1P, HORIZON_WEAPON_IDLE_FX_3P, "FX_CORE", true )
	weapon.PlayWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, HORIZON_WEAPON_LIGHT_FX_3P, "FX_LIGHT", true )
}

void function StartPermanentVFX1P( entity weapon )
{
	weapon.PlayWeaponEffect( HORIZON_WEAPON_BASE_FX_1P, $"", "FX_BASE", true )
	weapon.PlayWeaponEffect( HORIZON_WEAPON_IDLE_FX_1P, $"", "FX_CORE", true )
	weapon.PlayWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, $"", "FX_LIGHT", true )
}

void function StopPermanentVFX( entity weapon )
{
	weapon.StopWeaponEffect( HORIZON_WEAPON_IDLE_FX_1P, HORIZON_WEAPON_IDLE_FX_3P )
	weapon.StopWeaponEffect( HORIZON_WEAPON_BASE_FX_1P, HORIZON_WEAPON_BASE_FX_3P )
	weapon.StopWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, HORIZON_WEAPON_LIGHT_FX_3P )
}

void function StopPermanentVFX1P( entity weapon )
{
	weapon.StopWeaponEffect( HORIZON_WEAPON_IDLE_FX_1P, $"" )
	weapon.StopWeaponEffect( HORIZON_WEAPON_BASE_FX_1P, $"" )
	weapon.StopWeaponEffect( HORIZON_WEAPON_LIGHT_FX_1P, $"" )
}
                         