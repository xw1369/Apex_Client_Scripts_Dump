global function MpWeaponLmg_Init

global function OnWeaponActivate_weapon_lmg


global function OnWeaponPrimaryAttack_weapon_lmg


void function MpWeaponLmg_Init()
{
	BasicBoltPrecache()
}


void function OnWeaponActivate_weapon_lmg( entity weapon )
{

		UpdateViewmodelAmmo( false, weapon )

}


var function OnWeaponPrimaryAttack_weapon_lmg( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	GoldenHorsePurple_OnWeaponPrimaryAttack( weapon, attackParams )

	weapon.FireWeapon_Default( attackParams.pos, attackParams.dir, 1.0, 1.0, false )

	GoldenHorsePurple_PostFire( weapon )

	return weapon.GetWeaponSettingInt( eWeaponVar.ammo_per_shot )
}

