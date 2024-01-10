global function OnWeaponActivate_G7
global function OnWeaponDeactivate_G7
global function OnWeaponPrimaryAttack_G7
global function OnProjectileCollision_G7

void function OnWeaponActivate_G7( entity weapon )
{



}


void function OnWeaponDeactivate_G7( entity weapon )
{



}


var function OnWeaponPrimaryAttack_G7( entity weapon, WeaponPrimaryAttackParams attackParams )
{

		GoldenHorsePurple_OnWeaponPrimaryAttack( weapon, attackParams )


	weapon.FireWeapon_Default( attackParams.pos, attackParams.dir, 1.0, 1.0, false )


		GoldenHorsePurple_PostFire( weapon )


	return weapon.GetWeaponSettingInt( eWeaponVar.ammo_per_shot )
}


void function OnProjectileCollision_G7( entity projectile, vector pos, vector normal, entity hitEnt, int hitBox, bool isCritical, bool isPassthrough )
{



}