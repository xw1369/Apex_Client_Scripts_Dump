
global function AprilFools_S20_Init
global function IsAprilFools_S20
global function AprilFools_S20_Switcheroo
global function AprilFools_S20_FreeDMAirdropEnabled
global function AprilFools_S20_GunGameNPCEnabled
global function AprilFools_S20_ApplyWeaponGravityMod










const string PVAR_APRIL_FOOLS_S20_ENABLED = "is_april_fools_s20"
const string PVAR_APRIL_FOOLS_S20_GRAVITY_ENABLED = "april_fools_s20_gravity_enabled"
const string PVAR_APRIL_FOOLS_S20_WEAPON_GRAVITY_ENABLED = "april_fools_s20_weapon_gravity_enabled"
const string PVAR_APRIL_FOOLS_S20_WEAPON_GRAVITY_ULT_ENABLED = "april_fools_s20_weapon_gravity_ult_enabled"
const string PVAR_APRIL_FOOLS_S20_WEAPON_SPAWN_TYPE = "april_fools_s20_spawn_type"
const string PVAR_APRIL_FOOLS_S20_WEAPON_RESKIN_ENABLED = "april_fools_s20_reskin_enabled"
const string PVAR_APRIL_FOOLS_S20_CARE_PACKAGE_SPAWN_ENABLED = "april_fools_s20_care_package_spawn_enabled"
const string PVAR_APRIL_FOOLS_S20_NESSIE_PING_ENABLED = "april_fools_s20_nessie_ping_enabled"
const string PVAR_APRIL_FOOLS_S20_NESSIE_AMMO_COUNT = "april_fools_s20_nessie_ammo_count"
const string PVAR_APRIL_FOOLS_S20_FREEDM_AIRDROP_SPAWN_ENABLED = "april_fools_s20_freedm_airdrop_enabled"
const string PVAR_APRIL_FOOLS_S20_GUNGAME_NPC_ENABLED = "april_fools_s20_gungame_npc_enabled"


const string APRIL_FOOLS_S20_WEAPON_GRAVITY_MOD = "hopup_april_fools_gravity"
const string APRIL_FOOLS_S20_WEAPON_GRAVITY_VAR = "april_fools_force_scale"


enum eAprilFoolsWeaponSpawnType
{
	OFF = 0
	ONLY_SHOTGUN_PISTOL = 1
	ONLY_SHOTGUN_PISTOL_AND_MYTHIC = 2
	ONLY_MYTHIC = 3
	ALL_WEAPONS = 4
}

const string APRIL_FOOLS_S20_GRAVITY_MOD = "low_gravity_"
const array <string> APRIL_FOOLS_S20_GRAVITY_MOD_ARRAY = [
	APRIL_FOOLS_S20_GRAVITY_MOD + 1,
	APRIL_FOOLS_S20_GRAVITY_MOD + 2,
	APRIL_FOOLS_S20_GRAVITY_MOD + 3,
	APRIL_FOOLS_S20_GRAVITY_MOD + 4
]
const int APRIL_FOOLS_S20_MOD_COUNT = 4

const int APRIL_FOOLS_S20_NESSIE_AMMO_COUNT = 8

const string APRIL_FOOLS_S20_SHOTGUN_PISTOL_REF = "mp_weapon_shotgun_pistol"

const string APRIL_FOOLS_S20_ENERGY_REF = "mp_weapon_shotgun_pistol_energy"
const string APRIL_FOOLS_S20_HEAVY_REF = "mp_weapon_shotgun_pistol_heavy"
const string APRIL_FOOLS_S20_LIGHT_REF = "mp_weapon_shotgun_pistol_light"
const string APRIL_FOOLS_S20_SNIPER_REF = "mp_weapon_shotgun_pistol_sniper"
const string APRIL_FOOLS_S20_NESSIE_REF = "mp_weapon_shotgun_pistol_april_fools"

const array<string> APRIL_FOOLS_S20_WEAPON_ARRAY = [
	APRIL_FOOLS_S20_ENERGY_REF,
	APRIL_FOOLS_S20_HEAVY_REF,
	APRIL_FOOLS_S20_LIGHT_REF,
	APRIL_FOOLS_S20_SNIPER_REF,
	APRIL_FOOLS_S20_NESSIE_REF
]

bool function IsAprilFools_S20()
{
	return GetCurrentPlaylistVarBool( PVAR_APRIL_FOOLS_S20_ENABLED, false )
}

int function AprilFools_S20_WeaponSpawnType()
{
	return GetCurrentPlaylistVarInt( PVAR_APRIL_FOOLS_S20_WEAPON_SPAWN_TYPE, eAprilFoolsWeaponSpawnType.OFF )
}

bool function AprilFools_S20_WeaponReskinEnabled()
{
	return GetCurrentPlaylistVarBool( PVAR_APRIL_FOOLS_S20_WEAPON_RESKIN_ENABLED, true )
}

bool function AprilFools_S20_GravityEnabled()
{
	return GetCurrentPlaylistVarBool( PVAR_APRIL_FOOLS_S20_GRAVITY_ENABLED, true )
}

bool function AprilFools_S20_WeaponGravityEnabled()
{
	return GetCurrentPlaylistVarBool( PVAR_APRIL_FOOLS_S20_WEAPON_GRAVITY_ENABLED, true )
}

bool function AprilFools_S20_WeaponGravityUltEnabled()
{
	return GetCurrentPlaylistVarBool( PVAR_APRIL_FOOLS_S20_WEAPON_GRAVITY_ULT_ENABLED, true )
}

bool function AprilFools_S20_CarePackageSpawnEnabled()
{
	return GetCurrentPlaylistVarBool( PVAR_APRIL_FOOLS_S20_CARE_PACKAGE_SPAWN_ENABLED, false )
}

bool function AprilFools_S20_NessiePingEnabled()
{
	return GetCurrentPlaylistVarBool( PVAR_APRIL_FOOLS_S20_NESSIE_PING_ENABLED, true )
}

bool function AprilFools_S20_GunGameNPCEnabled()
{
	return IsAprilFools_S20() && GetCurrentPlaylistVarBool( PVAR_APRIL_FOOLS_S20_GUNGAME_NPC_ENABLED, true )
}

bool function AprilFools_S20_FreeDMAirdropEnabled()
{
	return IsAprilFools_S20() && GetCurrentPlaylistVarBool( PVAR_APRIL_FOOLS_S20_FREEDM_AIRDROP_SPAWN_ENABLED, true )
}


void function AprilFools_S20_Switcheroo( LootData wData )
{
	if ( !IsAprilFools_S20() )
		return

	if ( wData.lootType == eLootType.MAINWEAPON )
	{
		wData.baseMods.append( APRIL_FOOLS_S20_WEAPON_GRAVITY_MOD )
	}
}

void function AprilFools_S20_Init()
{
	if ( !IsAprilFools_S20() )
		return





















	if ( AprilFools_S20_WeaponGravityEnabled() )
	{



		AddCallback_OnWeaponAttack( OnWeaponAttack )
	}
}






















































































void function AprilFools_S20_ApplyWeaponGravityMod( entity weapon )
{
	if ( !IsAprilFools_S20() )
		return

	if ( !AprilFools_S20_WeaponGravityEnabled() )
		return

	if ( !AprilFools_S20_WeaponGravityUltEnabled() )
		return

	if ( !IsValid( weapon ) )
		return

	if ( weapon.HasMod( APRIL_FOOLS_S20_WEAPON_GRAVITY_MOD ) )
		return

	weapon.AddMod( APRIL_FOOLS_S20_WEAPON_GRAVITY_MOD )
}














void function OnWeaponAttack( entity player, entity weapon, string weaponName, int ammoUsed, vector attackOrigin, vector attackDir )
{

		if ( !InPrediction() )
			return


	if ( player.IsOnGround() )
		return

	if ( weapon.HasMod( APRIL_FOOLS_S20_WEAPON_GRAVITY_MOD ) )
	{
		int dmg               = weapon.GetWeaponSettingInt( eWeaponVar.damage_far_value )

		if ( weaponName == "mp_weapon_shotgun_pistol" && weapon.HasMod( WEAPON_LOCKEDSET_MOD_APRILFOOLS ) )
		{
			dmg = GetWeaponInfoFileKeyField_GlobalInt( "mp_weapon_shotgun_pistol", "damage_far_value" )
		}

		float forceScale      = GetWeaponInfoFileKeyField_GlobalFloat( weapon.GetWeaponClassName(), "april_fools_force_scale" )
		float force           = dmg * forceScale
		vector forceDirection = attackDir * -1
		vector finalForce     = force * forceDirection
		finalForce += player.GetVelocity()
		player.PlayerLaunch( finalForce, false )
	}
}

























































































































































































































