

global function MpAbilityCopycatKit_Init
global function CopycatKit_LootPickedUp
global function CopycatKit_LootDropped


global function CopycatKitChargePercent_Think
global function ServerCallback_PickupError


global const string COPYCAT_MOD = "copycat_mod"
global const string COPYCAT_NAME = "mp_ability_copycat_kit"
global const string COPYCAT_ABORT = "copycat_abort"



void function MpAbilityCopycatKit_Init()
{

	Remote_RegisterClientFunction( "ServerCallback_PickupError", "entity" )
	RegisterSignal( COPYCAT_ABORT )








		AddCallback_EditLootDesc( EditCopycatKitLootDescription )

}


void function CopycatKit_LootPickedUp ( entity player, entity pickup, string ref, int unitsPickedUp, bool willDestroy, entity deathBox, int pickupFlags )
{
	if ( ref.find( COPYCAT_NAME ) != 0 )
		return

	string kitAbilityRef = GetWeaponInfoFileKeyField_GlobalString ( ref, "disguise_ability" )

	if (kitAbilityRef == "")
		return

	array<entity> offhandWeapons = player.GetOffhandWeapons()

	
	
	
	foreach (offhand in offhandWeapons)
	{
		if (kitAbilityRef == offhand.GetWeaponClassName())
		{




			return
		}
	}







	entity costumeWeapon = player.GetOffhandWeapon( OFFHAND_GENERIC )

	
	costumeWeapon.AddMod( COPYCAT_MOD )













}


void function CopycatKit_LootDropped (entity player, string equipSlot, string equipRef, entity droppedEnt)
{

	droppedEnt.Signal( COPYCAT_ABORT )

































}

void function ServerCallback_PickupError ( entity player )
{

		AnnouncementMessageRight( GetLocalClientPlayer(), Localize( "#SURVIVAL_PICKUP_COPYCAT_KIT_DUPE_LEGEND_ERROR" ), "", <1, 0, 0>, $"", 1.0, "survival_ui_ability_notready" )

}


void function CopycatKitChargePercent_Think( entity player, entity weapon )
{
	AssertIsNewThread()
	player.EndSignal( "OnDeath" )
	weapon.EndSignal( "OnDestroy" )


	var dpadMenuRui = GetDpadMenuRui()

	RuiSetBool( dpadMenuRui, "gadgetUIEnabled", true )
	RuiSetBool( dpadMenuRui, "gadgetChargeUIEnabled", true )

	entity doppelWeapon 		= player.GetOffhandWeapon( OFFHAND_GENERIC )
	int ammoMinToFire 			= doppelWeapon.GetWeaponSettingInt( eWeaponVar.ammo_min_to_fire )
	float regenAmmoRate			= doppelWeapon.GetWeaponSettingFloat( eWeaponVar.regen_ammo_refill_rate )
	string kitAbilityRef		= GetWeaponInfoFileKeyField_GlobalString ( weapon.GetWeaponClassName(), "disguise_ability" )

	if ( kitAbilityRef != doppelWeapon.GetWeaponClassName())
		return

	while( true )
	{
		if ( IsValid( doppelWeapon ) && dpadMenuRui != null )
		{
			float tier1ChargeTimePercent = float( doppelWeapon.GetWeaponPrimaryClipCount() ) / float( doppelWeapon.GetWeaponPrimaryClipCountMax() ) * 100
			int tier1roundedPercent      = int( tier1ChargeTimePercent )

			RuiSetString( dpadMenuRui, "gadgetChargePercent", Localize( "#N_PERCENT", tier1roundedPercent ) )
			RuiSetFloat( dpadMenuRui, "gadgetChargeFrac", tier1ChargeTimePercent / 100.0 )
			RuiSetInt( dpadMenuRui, "ammoMinToFire", ammoMinToFire )
			RuiSetFloat( dpadMenuRui, "ammoRefillRate", regenAmmoRate )
		}
		WaitFrame()
	}

}



string function EditCopycatKitLootDescription( string lootRef, entity player, string originalDesc )
{
	string finalDesc = originalDesc

	if ( lootRef.find( COPYCAT_NAME ) == 0)
	{
		string kitAbilityRef = GetWeaponInfoFileKeyField_GlobalString ( lootRef, "disguise_ability" )

		string kitDescription = GetWeaponInfoFileKeyField_GlobalString ( lootRef, "description" )
		string copiedAbilityDescription

		if (lootRef == "mp_ability_copycat_kit_mirage_holopilot" )
		copiedAbilityDescription = Localize("#SURVIVAL_PICKUP_COPYCAT_KIT_HOLOPILOT_DESC")
		else
		copiedAbilityDescription = GetWeaponInfoFileKeyField_GlobalString ( kitAbilityRef, "description" )

		finalDesc =  Localize(kitDescription) + "\n \n" +  Localize(copiedAbilityDescription)
	}

	return finalDesc
}


                           