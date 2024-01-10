global function OnWeaponActivate_weapon_dmr
global function OnWeaponDeactivate_weapon_dmr
global function OnProjectileCollision_weapon_dmr


global function OnWeaponPrimaryAttack_weapon_dmr


global function MpWeaponDmr_Init

global function OnClientAnimEvent_weapon_dmr


void function OnWeaponActivate_weapon_dmr( entity weapon )
{



}

void function OnWeaponDeactivate_weapon_dmr( entity weapon )
{



}

void function OnProjectileCollision_weapon_dmr( entity projectile, vector pos, vector normal, entity hitEnt, int hitBox, bool isCritical, bool isPassthrough )
{



}


var function OnWeaponPrimaryAttack_weapon_dmr( entity weapon, WeaponPrimaryAttackParams attackParams )
{
	GoldenHorsePurple_OnWeaponPrimaryAttack( weapon, attackParams )

	weapon.FireWeapon_Default( attackParams.pos, attackParams.dir, 1.0, 1.0, false )

	GoldenHorsePurple_PostFire( weapon )

	return weapon.GetWeaponSettingInt( eWeaponVar.ammo_per_shot )
}


void function MpWeaponDmr_Init()
{
	DMRPrecache()
}

void function DMRPrecache()
{
	PrecacheParticleSystem( $"wpn_mflash_snp_hmn_smoke_side_FP" )
	PrecacheParticleSystem( $"wpn_mflash_snp_hmn_smoke_side" )
}


void function OnClientAnimEvent_weapon_dmr( entity weapon, string name )
{
	GlobalClientEventHandler( weapon, name )

	if ( name == "muzzle_flash" )
	{
		if ( IsOwnerViewPlayerFullyADSed( weapon ) )
			return
		if ( !weapon.HasMod( "barrel_stabilizer_l4_flash_hider" ) )
		{
			weapon.PlayWeaponEffect( $"wpn_mflash_snp_hmn_smoke_side_FP", $"wpn_mflash_snp_hmn_smoke_side", "muzzle_flash_L" )
			weapon.PlayWeaponEffect( $"wpn_mflash_snp_hmn_smoke_side_FP", $"wpn_mflash_snp_hmn_smoke_side", "muzzle_flash_R" )
		}
	}

	if ( name == "shell_eject" )
	{
		thread DelayedCasingsSound( weapon, 0.6 )
	}
}


void function DelayedCasingsSound( entity weapon, float delayTime )
{
	Wait( delayTime )

	if ( !IsValid( weapon ) )
		return

	weapon.EmitWeaponSound( "large_shell_drop" )
}
