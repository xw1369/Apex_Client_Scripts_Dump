
global function Pickup_XP_Cache_Init

global const string XPCACHE_BASIC_NAME = "mp_pickup_xp_cache"
global const string XPCACHE_ENHANCED_NAME = "mp_pickup_xp_cache_dynamic"
global const string XPCACHE_LARGE_NAME = "mp_pickup_xp_cache_large"

global enum eEvoCacheType
{
	EVO_CACHE_SMALL,
	EVO_CACHE_LARGE,
	EVO_CACHE_DYNAMIC,
	Count
}


void function Pickup_XP_Cache_Init()
{

		RegisterCustomItemPickupAction( XPCACHE_BASIC_NAME, XPCache_ItemPickup )
		RegisterCustomItemPickupAction( XPCACHE_LARGE_NAME, XPCache_ItemPickup )
		RegisterCustomItemPickupAction( XPCACHE_ENHANCED_NAME, XPCache_ItemPickup )

}


bool function XPCache_ItemPickup( entity pickup, entity player, int pickupFlags, entity deathBox, int ornull desiredCount, LootData data )
{



















	return true
}

                          