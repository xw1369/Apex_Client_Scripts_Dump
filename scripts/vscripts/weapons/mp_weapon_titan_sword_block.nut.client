
global function MpWeaponTitanSword_Block_Init
global function TitanSword_Block_OnWeaponActivate
global function TitanSword_Block_ClearMods

global function TitanSword_Block_PlayerIsBlocking
global function TitanSword_Block_IsBlocking


const string TITAN_SWORD_BLOCK_MOD = "blocking"


const string SIG_TITAN_SWORD_BLOCK_DEACTIVATE = "TitanSword_DeactivateBlock"


const asset VFX_TITAN_SWORD_BLOCK = $"P_xo_sword_block"
const asset VFX_TITAN_SWORD_BLOCK_HIT_1P = $"P_xo_sword_block_hit"
const asset VFX_TITAN_SWORD_BLOCK_HIT_3P = $"P_xo_sword_block_hit_3P"
const asset VFX_TITAN_SWORD_BLOCK_BULLET_HIT = $"P_pilot_sword_block_bullet"
const asset VFX_TITAN_SWORD_BLOCK_SWORD_HIT = $"P_pilot_sword_block_sword"


const string SFX_TITAN_SWORD_BLOCK_DAMAGE = "titansword_block_bullet_impacts_1p"

struct
{
}file

void function MpWeaponTitanSword_Block_Init()
{
	PrecacheParticleSystem( VFX_TITAN_SWORD_BLOCK_HIT_1P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_BLOCK_HIT_3P )
	PrecacheParticleSystem( VFX_TITAN_SWORD_BLOCK_BULLET_HIT )
	PrecacheParticleSystem( VFX_TITAN_SWORD_BLOCK_SWORD_HIT )






}


void function TitanSword_Block_OnWeaponActivate( entity player, entity weapon )
{





}

void function TitanSword_Block_ClearMods( entity weapon )
{
	weapon.RemoveMod( TITAN_SWORD_BLOCK_MOD )
}

bool function TitanSword_Block_PlayerIsBlocking( entity player )
{
	if ( !player.IsPlayer() )
		return false

	entity weapon = TitanSword_GetMainWeapon( player )
	if ( IsValid( weapon ) )
		return TitanSword_Block_IsBlocking( weapon )

	return false
}

bool function TitanSword_Block_IsBlocking( entity weapon )
{
	return weapon.IsWeaponInAds()
}























































































































































































































































































                               