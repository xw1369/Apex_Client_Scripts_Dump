
global function MpWeaponShotgunPistol_Init
global function OnWeaponActivate_weapon_shotgun_pistol
global function OnWeaponDeactivate_weapon_shotgun_pistol
global function OnWeaponAkimboStateChanged_weapon_shotgun_pistol

global function OnWeaponPrimaryAttack_weapon_shotgun_pistol
global function OnProjectileCollision_weapon_shotgun_pistol










void function MpWeaponShotgunPistol_Init()
{
}


var function OnWeaponPrimaryAttack_weapon_shotgun_pistol( entity weapon, WeaponPrimaryAttackParams attackParams )
{









	bool playerFired = true
	return Fire_ShotgunPistol( weapon, attackParams, playerFired )
}









int function Fire_ShotgunPistol( entity weapon, WeaponPrimaryAttackParams attackParams, bool playerFired = true )
{
	float patternScale = 1.0
	if ( playerFired )
	{
		
		entity owner             = weapon.GetWeaponOwner()
		float maxAdsPatternScale = expect float( weapon.GetWeaponInfoFileKeyField( "blast_pattern_ads_scale" ) )
		patternScale *= GraphCapped( owner.GetZoomFrac(), 0.0, 1.0, 1.0, maxAdsPatternScale )
	}
	else
	{
		patternScale = weapon.GetWeaponSettingFloat( eWeaponVar.blast_pattern_npc_scale )
	}

	float speedScale  = 1.0
	bool ignoreSpread = true




















	weapon.FireWeapon_Default( attackParams.pos, attackParams.dir, speedScale, patternScale, ignoreSpread )

	return weapon.GetWeaponSettingInt( eWeaponVar.ammo_per_shot )
}

void function OnProjectileCollision_weapon_shotgun_pistol( entity projectile, vector pos, vector normal, entity hitEnt, int hitbox, bool isCritical, bool isPassthrough )
{








}
























































































void function UpdateAkimboMozamShotgunBoltPairing( entity weapon )
{
	if ( !IsValid( weapon ) )
		return

	bool isPredictedOrServer = InPrediction() && IsFirstTimePredicted()



	if ( !isPredictedOrServer )
		return

	const string BOLT_MOD_BASE_2 = "shotgun_bolt_"
	const string AKIMBO_SUFFIX = "_akimbo_active"
	array<string> levels = ["l1", "l2", "l3"]

		levels.append( "l4" )


	
	
	if ( weapon.HasMod( "akimbo_active" ) )
	{

		foreach ( string level in levels )
		{
			string boltModBase = BOLT_MOD_BASE_2 + level
			string boltModDT = boltModBase + AKIMBO_SUFFIX

			weapon.RemoveMod( boltModDT )

			if ( weapon.HasMod( boltModBase ) )
				weapon.AddMod( boltModDT )
		}
	}
	else
	{
		foreach ( string level in levels )
		{
			string boltModAkimbo = BOLT_MOD_BASE_2 + level + AKIMBO_SUFFIX

			if ( !IsValid( weapon ) )
				return

			weapon.RemoveMod( boltModAkimbo )
		}
	}
}
void function OnWeaponActivate_weapon_shotgun_pistol( entity weapon )
{
	OnWeaponActivate_weapon_basic_bolt( weapon )

	











}

void function OnWeaponDeactivate_weapon_shotgun_pistol( entity weapon )
{
	
}

void function OnWeaponAkimboStateChanged_weapon_shotgun_pistol( entity weapon, entity player, int currentAkimboState )
{

		if ( player != GetLocalViewPlayer() || weapon.IsAkimboAlthand() )
			return

		UpdateHudDataForMainWeapons( player, weapon )

		var weaponRui = GetWeaponRui()
		OnPrimaryWeaponStatusUpdate_Akimbo( weapon, weaponRui )

}
               