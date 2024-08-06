










global function UICallback_RTKPopulateSquadGladCards
global function UICallback_RTKToggleMutePings
global function UICallback_RTKBlockSquadMate


global struct RTKSquadGladCards_Properties
{
	rtk_panel gladCardList
}


global struct RTKSquadGladCardModel
{
	string uid
	string unspoofedUid
	string eaid
	string hardware
	int    team
	string id = ""
	string name = ""

	bool canMute = false
	bool isMuted = false
	bool canViewProfile = false

	bool canMutePing = false
	bool isPingMuted = false

	bool canReport = false
	bool canBlock = false
	bool canInvite = false
	bool canSendFriendRequest = false
	bool isConnected = true
	bool isLocalPlayer = true

	string actionText = ""

	bool isFriendly = false
}






































































































































































































































































void function UICallback_RTKBlockSquadMate( string path, int index )
{
	rtk_struct model = RTKDataModel_GetStruct( path )
	rtk_array squadGladCardsModel = RTKStruct_GetArray( model, "gladcards" )
	int squadGladCardsModelCount = RTKArray_GetCount( squadGladCardsModel )

	if( squadGladCardsModelCount > index )
	{
		rtk_struct quadGladCardModel = RTKArray_GetStruct( squadGladCardsModel, index )

		string uid = RTKStruct_GetString( quadGladCardModel, "uid" )
		string eaid = RTKStruct_GetString( quadGladCardModel, "eaid" )

		array< PlayerInfo > teammates = Cl_Squad_GetTeammates()
		PlayerInfo playerData
		foreach ( data in teammates )
		{
			if( data.uid == uid && data.eaid == eaid )
			{
				playerData = data
				break
			}
		}

		printt( "#EADP UICallback_BlockSquadMate", playerData.ehi, playerData.hardware, playerData.uid, playerData.eaid )
		RunUIScript( "ClientToUI_ShowBlockPlayerDialog", GetDisplayablePlayerNameFromEHI( playerData.ehi ), playerData.hardware, playerData.uid, playerData.eaid )
	}
}




































































void function UICallback_RTKToggleMutePings( string path, int index )
{
	rtk_struct model = RTKDataModel_GetStruct( path )
	rtk_array squadGladCardsModel = RTKStruct_GetArray( model, "gladcards" )
	int squadGladCardsModelCount = RTKArray_GetCount( squadGladCardsModel )

	if( squadGladCardsModelCount > index )
	{
		rtk_struct quadGladCardModel = RTKArray_GetStruct( squadGladCardsModel, index )

		string uid = RTKStruct_GetString( quadGladCardModel, "uid" )
		string eaid = RTKStruct_GetString( quadGladCardModel, "eaid" )

		array< PlayerInfo > teammates = Cl_Squad_GetTeammates()
		entity player
		foreach ( data in teammates )
		{
			if( data.uid == uid && data.eaid == eaid )
			{
				player = FromEHI( data.ehi )
				break
			}
		}

		if( IsValid( player ) )
		{
			TogglePlayerWaypointMute( player )

			RTKStruct_SetBool( quadGladCardModel, "isPingMuted", PlayerIsPingMuted( player ) )
		}
	}
}













































void function UICallback_RTKPopulateSquadGladCards( string path )
{
	entity localPlayer = GetLocalClientPlayer()
	EHI localPlayerEHI = ToEHI( localPlayer )

	rtk_struct model = RTKDataModel_GetStruct( path )
	rtk_array squadGladCardModel = RTKStruct_GetOrCreateScriptArrayOfStructs( model, "gladcards", "RTKSquadGladCardModel" )
	array< PlayerInfo > teammates = Cl_Squad_GetTeammates( true )
	teammates = teammates.slice( 0, int( min( GetMaxTeamSizeForPlaylist( GetCurrentPlaylistName() ), teammates.len() ) ) )

	array<RTKSquadGladCardModel> dataModel

	foreach ( data in teammates )
	{
		entity dataPlayer = FromEHI( data.ehi )
		bool isLocalPlayer = data.ehi == localPlayerEHI
		bool isPlayerBot = false
		RTKSquadGladCardModel gladCard

		bool canReport = false
		if ( IsValid( dataPlayer ) )
		{
#if AUTO_PLAYER
				isPlayerBot 		  = AutoPlayer_IsAutoPlayer( dataPlayer ) || dataPlayer.IsBot()
#else
				isPlayerBot 		  = dataPlayer.IsBot()
#endif

			isPlayerBot 		  = dataPlayer.IsBot()
			gladCard.isMuted      = dataPlayer.IsVoiceAndTextMuted()
			gladCard.isPingMuted  = PlayerIsPingMuted( dataPlayer )
			gladCard.unspoofedUid = dataPlayer.GetUnspoofedPlatformUserIdStr()
			canReport = CanReportPlayer( dataPlayer )
		}
		else
		{
			int reportStyle = GetReportStyle()

			switch ( reportStyle )
			{
				case 1: 
					canReport = CrossplayUserOptIn() ? true : data.hardware == GetLocalClientPlayer().GetHardwareName()
				break

				case 2: 
					canReport = true
				break

				case 0: 
				default:
					canReport = false
			}
		}

		if( !isLocalPlayer && !isPlayerBot )
		{
			EHIScriptStruct teammateEHISS
			if ( EEHHasValidScriptStruct( data.ehi ) )
				teammateEHISS = GetEHIScriptStruct( data.ehi )

			if ( GetCurrentPlaylistVarBool( "showPlayerObfuscatedID", true ) && teammateEHISS.obfuscatedID != "" )
			{
				gladCard.id = Localize( "#DEATH_SCREEN_ID", teammateEHISS.obfuscatedID )
			}

			bool canAddFriend = CanSendFriendRequest( GetLocalClientPlayer() ) && !EADP_IsFriendByEAID( data.eaid )
			bool canInviteParty = CanInviteSquadMate( data.uid ) && CanInviteToparty() == 0

			if ( canAddFriend )
			{
				
				CommunityFriends friends = GetFriendInfo()
				foreach ( id in friends.ids )
				{
					if ( data.uid == id )
					{
						canAddFriend = false
						break
					}
				}
			}

			gladCard.canViewProfile = Teams_CanViewProfile( data.uid, data.hardware )
			string actionText = ""
			if( gladCard.canViewProfile )
				actionText += ( actionText == "" )? Localize( "#Y_BUTTON_VIEW_PROFILE" ): "\n" + Localize( "#Y_BUTTON_VIEW_PROFILE" )

			if( canInviteParty )
				actionText += ( actionText == "" )? Localize( "#CLICK_INVITE_PARTY" ): "\n" + Localize( "#CLICK_INVITE_PARTY" )

			if( canAddFriend )
				actionText += ( actionText == "" )? Localize( "#RCLICK_INVITE_FRIEND" ): "\n" + Localize( "#RCLICK_INVITE_FRIEND" )

			gladCard.actionText = Localize( actionText )
		}

		gladCard.isConnected 		  = isPlayerBot || ( ( IsValid( dataPlayer ) )? dataPlayer.IsConnectionActive(): false )
		gladCard.canBlock 			  = !isPlayerBot && !isLocalPlayer && CrossplayUserOptIn() && !IsUserOnSamePlatform( data.hardware )
		gladCard.canInvite            = !isPlayerBot && !isLocalPlayer
		gladCard.canMute              = !isPlayerBot && !isLocalPlayer && gladCard.isConnected
		gladCard.canMutePing          = !isPlayerBot && !isLocalPlayer && gladCard.isConnected
		gladCard.canReport            = !isPlayerBot && !isLocalPlayer && canReport
		gladCard.canSendFriendRequest = !isPlayerBot && !isLocalPlayer
		gladCard.name                 = GetDisplayablePlayerNameFromEHI( data.ehi )
		gladCard.team                 = data.team
		gladCard.uid                  = data.uid
		gladCard.eaid                 = data.eaid
		gladCard.hardware             = data.hardware
		gladCard.isFriendly           = isLocalPlayer || ( data.team == localPlayer.GetTeam() )
		gladCard.isLocalPlayer        = isLocalPlayer

		dataModel.append( gladCard )
	}

	RTKArray_SetValue( squadGladCardModel, dataModel )
}








