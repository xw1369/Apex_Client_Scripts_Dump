
global function MpWeaponLSTAR_Init

global function OnWeaponPrimaryAttack_weapon_lstar
global function OnWeaponCooldown_weapon_lstar
global function OnWeaponActivate_weapon_lstar
global function OnWeaponDeactivate_weapon_lstar


global function OnWeaponStartZoomIn_weapon_lstar
global function OnWeaponStartZoomOut_weapon_lstar
global function OnWeaponOwnerChanged_weapon_lstar







const LSTAR_BURNOUT_EFFECT_1P = $"P_lstar_burnout_sparks_FP"
const LSTAR_BURNOUT_EFFECT_3P = $"P_lstar_burnout_sparks"

const float LSTAR_OVERHEAT_WARNING_CHARGE_FRAC = 0.8

const string LSTAR_WARNING_SOUND_1P = "lstar_lowammowarning"
const string LSTAR_BURNOUT_SOUND_1P = "LSTAR_LensBurnout"
const string LSTAR_BURNOUT_SOUND_3P = "LSTAR_LensBurnout_3P"

void function MpWeaponLSTAR_Init()
{
	PrecacheParticleSystem( LSTAR_BURNOUT_EFFECT_1P )
	PrecacheParticleSystem( LSTAR_BURNOUT_EFFECT_3P )
}

var function OnWeaponPrimaryAttack_weapon_lstar( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	return LSTARPrimaryAttack( weapon, attackParams, true )
}








int function LSTARPrimaryAttack( entity weapon, WeaponPrimaryAttackParams attackParams, bool isPlayerFired )
{
	entity owner = weapon.GetOwner()


	if ( !weapon.ShouldPredictProjectiles() )
		return 1

	
	{
		if ( weapon.GetWeaponChargeFraction() >= LSTAR_OVERHEAT_WARNING_CHARGE_FRAC )
		{
			
			if ( IsValid( owner ) && (Time() - weapon.w.lastFireTime >= GetSoundDuration( LSTAR_WARNING_SOUND_1P ) ) )
			{
				weapon.w.lastFireTime = Time()
				EmitSoundOnEntity( owner, LSTAR_WARNING_SOUND_1P )
			}
		}
	}


	float speedScale = 1.0
	float patternScale = 1.0
	weapon.FireWeapon_Default( attackParams.pos, attackParams.dir, speedScale, patternScale, false )

	return weapon.GetWeaponSettingInt( eWeaponVar.ammo_per_shot )
}


void function OnWeaponCooldown_weapon_lstar( entity weapon, bool isFirstTimeCooldown )
{
	
	if ( isFirstTimeCooldown )
	{
		weapon.EmitWeaponSound_1p3p( "LSTAR_VentCooldown", "LSTAR_VentCooldown_3p" )

		
		if ( weapon.IsOverheated() )
		{
			weapon.EmitWeaponSound_1p3p( LSTAR_BURNOUT_SOUND_1P, LSTAR_BURNOUT_SOUND_3P )
			weapon.PlayWeaponEffect( LSTAR_BURNOUT_EFFECT_1P, LSTAR_BURNOUT_EFFECT_3P, "vent_spinner" )



				GoldenHorseGreen_OnWeaponReload( weapon, 0 )







		}
	}
}


void function OnWeaponStartZoomIn_weapon_lstar( entity weapon )
{
	HopupGunshield_OnWeaponStartZoomIn( weapon )
}

void function OnWeaponStartZoomOut_weapon_lstar( entity weapon )
{
	HopupGunshield_OnWeaponStartZoomOut( weapon )
}

void function OnWeaponOwnerChanged_weapon_lstar( entity weapon, WeaponOwnerChangedParams changeParams )
{

}





































void function OnWeaponActivate_weapon_lstar( entity weapon )
{
	entity owner = weapon.GetOwner()
	if ( !owner.IsPlayer() )
		return










		GoldenHorseGreen_OnWeaponActivate( weapon )



		HopupGunshield_OnWeaponActivate( weapon )

}

void function OnWeaponDeactivate_weapon_lstar( entity weapon )
{







		HopupGunshield_OnWeaponDeactivate( weapon )

}
