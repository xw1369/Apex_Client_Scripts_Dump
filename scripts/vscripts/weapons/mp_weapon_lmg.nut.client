global function MpWeaponLmg_Init

global function OnWeaponActivate_weapon_lmg
global function OnWeaponDeactivate_weapon_lmg


global function OnWeaponStartZoomIn_weapon_lmg
global function OnWeaponStartZoomOut_weapon_lmg
global function OnWeaponOwnerChanged_weapon_lmg



global function OnWeaponPrimaryAttack_weapon_lmg


void function MpWeaponLmg_Init()
{
	BasicBoltPrecache()
}


void function OnWeaponActivate_weapon_lmg( entity weapon )
{







		UpdateViewmodelAmmo( false, weapon )



		HopupGunshield_OnWeaponActivate( weapon )

}


var function OnWeaponPrimaryAttack_weapon_lmg( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	GoldenHorsePurple_OnWeaponPrimaryAttack( weapon, attackParams )

	weapon.FireWeapon_Default( attackParams.pos, attackParams.dir, 1.0, 1.0, false )

	GoldenHorsePurple_PostFire( weapon )

	return weapon.GetWeaponSettingInt( eWeaponVar.ammo_per_shot )
}


void function OnWeaponDeactivate_weapon_lmg( entity weapon )
{







		HopupGunshield_OnWeaponDeactivate( weapon )

}


void function OnWeaponStartZoomIn_weapon_lmg( entity weapon )
{
	HopupGunshield_OnWeaponStartZoomIn( weapon )
}

void function OnWeaponStartZoomOut_weapon_lmg( entity weapon )
{
	HopupGunshield_OnWeaponStartZoomOut( weapon )
}

void function OnWeaponOwnerChanged_weapon_lmg( entity weapon, WeaponOwnerChangedParams changeParams )
{

}
      