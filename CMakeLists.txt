cmake_minimum_required(VERSION 2.8)

include(CryptoNoteWallet.cmake)


project(${CN_PROJECT_NAME})

execute_process(COMMAND git log -1 --pretty=format:%h
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
OUTPUT_VARIABLE GIT_REVISION)

set(CRYPTONOTE_LIB cryptonote)
set (CMAKE_PREFIX_PATH "/Users/travis/Qt5.11.1/5.11.1/")

include_directories(${CMAKE_BINARY_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}
    src
    pennykoin/external
    pennykoin/include
    pennykoin/src)

find_package(Qt5Gui REQUIRED)
find_package(Qt5Widgets REQUIRED)
find_package(Qt5Network REQUIRED)

if(STATIC)
  set(Boost_USE_STATIC_LIBS ON)
  set(Boost_USE_STATIC_RUNTIME ON)
endif()

#set(Boost_DEBUG on)
find_package(Boost 1.53 REQUIRED COMPONENTS system filesystem date_time chrono regex serialization program_options)
include_directories(SYSTEM ${Boost_INCLUDE_DIRS})
find_package( Threads )
if(MINGW)
  set(Boost_LIBRARIES "${Boost_LIBRARIES};ws2_32;mswsock")
elseif(APPLE)
  set(Boost_LIBRARIES "${Boost_LIBRARIES}")
elseif(NOT MSVC)
  set(Boost_LIBRARIES "${Boost_LIBRARIES};rt")
endif()

set(VERSION "")
configure_file("pennykoin/src/version.h.in" "version.h")
configure_file("src/CryptoNoteWalletConfig.h.in" "CryptoNoteWalletConfig.h")

add_definitions(-DVERSION=\"${CN_VERSION}\")
add_definitions(-DGIT_REVISION=\"${GIT_REVISION}\")

set(CMAKE_AUTOMOC ON)

set(CRYPTONOTE_SOURCES
  pennykoin/external/miniupnpc/connecthostport.c
  pennykoin/external/miniupnpc/igd_desc_parse.c
  pennykoin/external/miniupnpc/minisoap.c
  pennykoin/external/miniupnpc/miniupnpc.c
  pennykoin/external/miniupnpc/miniwget.c
  pennykoin/external/miniupnpc/minixml.c
  pennykoin/external/miniupnpc/portlistingparse.c
  pennykoin/external/miniupnpc/receivedata.c
  pennykoin/external/miniupnpc/upnpcommands.c
  pennykoin/external/miniupnpc/upnpreplyparse.c
  pennykoin/src/Common/Base58.cpp
  pennykoin/src/Common/CommandLine.cpp
  pennykoin/src/Common/Util.cpp
  pennykoin/src/Common/StringTools.cpp
  pennykoin/src/Common/JsonValue.cpp
  pennykoin/src/Common/ConsoleTools.cpp
  pennykoin/src/Common/MemoryInputStream.cpp
  pennykoin/src/Common/PathTools.cpp
  pennykoin/src/Common/StdInputStream.cpp
  pennykoin/src/Common/StdOutputStream.cpp
  pennykoin/src/Common/StreamTools.cpp
  pennykoin/src/Common/StringOutputStream.cpp
  pennykoin/src/Common/StringView.cpp
  pennykoin/src/Common/VectorOutputStream.cpp
  pennykoin/src/crypto/blake256.c
  pennykoin/src/crypto/chacha.c
  pennykoin/src/crypto/crypto-ops-data.c
  pennykoin/src/crypto/crypto-ops.c
  pennykoin/src/crypto/crypto.cpp
  pennykoin/src/crypto/groestl.c
  pennykoin/src/crypto/hash-extra-blake.c
  pennykoin/src/crypto/hash-extra-groestl.c
  pennykoin/src/crypto/hash-extra-jh.c
  pennykoin/src/crypto/hash-extra-skein.c
  pennykoin/src/crypto/hash.c
  pennykoin/src/crypto/jh.c
  pennykoin/src/crypto/keccak.c
  pennykoin/src/crypto/oaes_lib.c
  pennykoin/src/crypto/random.c
  pennykoin/src/crypto/skein.c
  pennykoin/src/crypto/slow-hash.c
  pennykoin/src/crypto/slow-hash.cpp
  pennykoin/src/crypto/tree-hash.c
  pennykoin/src/CryptoNoteCore/BlockchainIndices.cpp
  pennykoin/src/CryptoNoteCore/BlockchainMessages.cpp
  pennykoin/src/CryptoNoteCore/BlockIndex.cpp
  pennykoin/src/CryptoNoteCore/CoreConfig.cpp
  pennykoin/src/CryptoNoteCore/CryptoNoteBasic.cpp
  pennykoin/src/CryptoNoteCore/CryptoNoteTools.cpp
  pennykoin/src/CryptoNoteCore/Currency.cpp
  pennykoin/src/CryptoNoteCore/DepositIndex.cpp
  pennykoin/src/CryptoNoteCore/MinerConfig.cpp
  pennykoin/src/CryptoNoteCore/Transaction.cpp
  pennykoin/src/CryptoNoteCore/Account.cpp
  pennykoin/src/CryptoNoteCore/Blockchain.cpp
  pennykoin/src/CryptoNoteCore/Checkpoints.cpp
  pennykoin/src/CryptoNoteCore/CryptoNoteBasicImpl.cpp
  pennykoin/src/CryptoNoteCore/Core.cpp
  pennykoin/src/CryptoNoteCore/CryptoNoteFormatUtils.cpp
  pennykoin/src/CryptoNoteCore/CryptoNoteSerialization.cpp
  pennykoin/src/CryptoNoteCore/Difficulty.cpp
  pennykoin/src/CryptoNoteCore/IBlock.cpp
  pennykoin/src/CryptoNoteCore/Miner.cpp
  pennykoin/src/CryptoNoteCore/TransactionExtra.cpp
  pennykoin/src/CryptoNoteCore/TransactionPool.cpp
  pennykoin/src/CryptoNoteCore/TransactionPrefixImpl.cpp
  pennykoin/src/CryptoNoteCore/TransactionUtils.cpp
  pennykoin/src/InProcessNode/InProcessNode.cpp
  pennykoin/src/InProcessNode/InProcessNodeErrors.cpp
  pennykoin/src/NodeRpcProxy/NodeErrors.cpp
  pennykoin/src/NodeRpcProxy/NodeRpcProxy.cpp
  pennykoin/src/P2p/NetNodeConfig.cpp
  pennykoin/src/Serialization/BinaryInputStreamSerializer.cpp
  pennykoin/src/Serialization/BinaryOutputStreamSerializer.cpp
  pennykoin/src/Serialization/JsonInputValueSerializer.cpp
  pennykoin/src/Serialization/JsonOutputStreamSerializer.cpp
  pennykoin/src/Serialization/KVBinaryInputStreamSerializer.cpp
  pennykoin/src/Serialization/KVBinaryOutputStreamSerializer.cpp
  pennykoin/src/Serialization/SerializationOverloads.cpp
  pennykoin/src/Transfers/BlockchainSynchronizer.cpp
  pennykoin/src/Transfers/SynchronizationState.cpp
  pennykoin/src/Transfers/TransfersConsumer.cpp
  pennykoin/src/Transfers/TransfersContainer.cpp
  pennykoin/src/Transfers/TransfersSubscription.cpp
  pennykoin/src/Transfers/TransfersSynchronizer.cpp
  pennykoin/src/Wallet/LegacyKeysImporter.cpp
  pennykoin/src/Wallet/WalletAsyncContextCounter.cpp
  pennykoin/src/Wallet/WalletErrors.cpp
  pennykoin/src/WalletLegacy/KeysStorage.cpp
  pennykoin/src/WalletLegacy/WalletLegacy.cpp
  pennykoin/src/WalletLegacy/WalletHelper.cpp
  pennykoin/src/WalletLegacy/WalletLegacySerializer.cpp
  pennykoin/src/WalletLegacy/WalletLegacySerialization.cpp
  pennykoin/src/WalletLegacy/WalletTransactionSender.cpp
  pennykoin/src/WalletLegacy/WalletUnconfirmedTransactions.cpp
  pennykoin/src/WalletLegacy/WalletUserTransactionsCache.cpp
  pennykoin/src/System/ContextGroup.cpp
  pennykoin/src/System/Event.cpp
  pennykoin/src/System/EventLock.cpp
  pennykoin/src/System/InterruptedException.cpp
  pennykoin/src/System/Ipv4Address.cpp
  pennykoin/src/System/TcpStream.cpp
  pennykoin/src/HTTP/HttpRequest.cpp
  pennykoin/src/HTTP/HttpParser.cpp
  pennykoin/src/HTTP/HttpParserErrorCodes.cpp
  pennykoin/src/HTTP/HttpResponse.cpp
  pennykoin/src/Rpc/HttpClient.cpp
  pennykoin/src/Rpc/JsonRpc.cpp
  pennykoin/src/P2p/NetNode.cpp
  pennykoin/src/P2p/LevinProtocol.cpp
  pennykoin/src/P2p/NetNodeConfig.cpp
  pennykoin/src/P2p/PeerListManager.cpp
  pennykoin/src/CryptoNoteProtocol/CryptoNoteProtocolHandler.cpp
  pennykoin/src/Logging/ILogger.cpp
  pennykoin/src/Logging/LoggerGroup.cpp
  pennykoin/src/Logging/CommonLogger.cpp
  pennykoin/src/Logging/LoggerManager.cpp
  pennykoin/src/Logging/FileLogger.cpp
  pennykoin/src/Logging/StreamLogger.cpp
  pennykoin/src/Logging/ConsoleLogger.cpp
  pennykoin/src/Logging/LoggerMessage.cpp
  pennykoin/src/Logging/LoggerRef.cpp
  pennykoin/src/BlockchainExplorer/BlockchainExplorer.cpp
  pennykoin/src/BlockchainExplorer/BlockchainExplorerDataBuilder.cpp
  pennykoin/src/BlockchainExplorer/BlockchainExplorerErrors.cpp
)

file(GLOB_RECURSE SOURCES src/*.cpp)
file(GLOB_RECURSE HEADERS src/*.h)
file(GLOB_RECURSE FORMS src/gui/ui/*.ui)

set(QRC src/resources.qrc)

qt5_wrap_ui(UIS ${FORMS})
qt5_add_resources(RCC ${QRC})


if (WIN32)
  if (NOT MSVC)
	message(FATAL_ERROR "Only MSVC is supported on this platform")
  endif ()
  add_definitions(/D_CRT_SECURE_NO_WARNINGS /D_WIN32_WINNT=0x0600 /DSTATICLIB)
  include_directories(pennykoin/src/platform/msc)

  set(PLATFORM_DIR Windows)
  set(BUILD_PLATFORM WIN32)
  set(BUILD_RESOURCES src/cryptonotewallet.rc)
  set(QTMAIN Qt5::WinMain)

elseif (UNIX)
  set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} pennykoin/external/miniupnpc/minissdpc.c)
  if (APPLE)
    enable_language(ASM)
    file(GLOB_RECURSE OBJC_SOURCES src/*.mm)
    set(SOURCES ${SOURCES} ${OBJC_SOURCES})
    set(PLATFORM_DIR OSX)
    set(MACOSX_BUNDLE_INFO_STRING "Cryptonote GUI wallet")
    set(MACOSX_BUNDLE_LONG_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_NAME CryptonoteWallet)
    set(MACOSX_BUNDLE_SHORT_VERSION_STRING "${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")
    set(MACOSX_BUNDLE_BUNDLE_VERSION "$${VERSION_VERSION}.${VERSION_MINOR}.${VERSION_PATCH}")

    find_package(Qt5PrintSupport REQUIRED)

    include_directories(/usr/include/malloc)

    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11 -stdlib=libc++")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c11 -maes -D_DARWIN_C_SOURCE")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -framework Cocoa -framework OpenGL -framework CoreFoundation -framework Carbon -framework IOKit -L/usr/lib")

    set(MACOSX_BUNDLE_ICON_FILE cryptonote.icns)
    set(APPLICATION_ICON src/images/cryptonote.icns)
    set_source_files_properties(${APPLICATION_ICON} PROPERTIES MACOSX_PACKAGE_LOCATION "Resources")

    set(BUILD_PLATFORM MACOSX_BUNDLE)
    set(BUILD_RESOURCES ${APPLICATION_ICON})
	
    GET_TARGET_PROPERTY(QT_LIB_DIR "${Qt5Widgets_LIBRARIES}" LOCATION)
    GET_FILENAME_COMPONENT(QT_LIB_DIR "${QT_LIB_DIR}" PATH)
  else(APPLE)
    set(PLATFORM_DIR Linux)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -maes -std=c++11")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -maes -std=c11")
  endif (APPLE)
endif ()

include_directories(pennykoin/src/Platform/${PLATFORM_DIR})
file(GLOB PLATFORM_SOURCES pennykoin/src/Platform/${PLATFORM_DIR}/System/*)
set(CRYPTONOTE_SOURCES ${CRYPTONOTE_SOURCES} ${PLATFORM_SOURCES})

add_library(${CRYPTONOTE_LIB} STATIC ${CRYPTONOTE_SOURCES})
set_target_properties(${CRYPTONOTE_LIB} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${CRYPTONOTE_LIB} ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BUILD_PLATFORM} ${BUILD_RESOURCES} ${SOURCES} ${HEADERS} ${UIS} ${RCC})
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_DEFINITIONS _GNU_SOURCE)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${QTMAIN} ${CRYPTONOTE_LIB})

if (APPLE)
  qt5_use_modules(${PROJECT_NAME} PrintSupport)
elseif (UNIX)
  target_link_libraries(${PROJECT_NAME} -lpthread)
elseif (WIN32)
  target_link_libraries(${PROJECT_NAME} Imm32 Iphlpapi Winmm)
endif (APPLE)

qt5_use_modules(${PROJECT_NAME} Widgets Gui Network)

# Installation

set(CPACK_PACKAGE_NAME ${CN_PROJECT_NAME})
set(CPACK_PACKAGE_VERSION ${CN_VERSION})
set(CPACK_PACKAGE_VENDOR "Pennykoin")
set(CPACK_PACKAGE_CONTACT "https://pennykoin.com/")
set(CPACK_STRIP_FILES ON)
if (APPLE)
    set(CPACK_GENERATOR DragNDrop)
    install(TARGETS ${PROJECT_NAME} BUNDLE DESTINATION .)
    set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}.-${VERSION_RELEASE}")
elseif (UNIX)
    install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION bin)
    install(FILES src/QTpenny.desktop DESTINATION share/applications)
    install(FILES src/images/cryptonote.png DESTINATION share/pixmaps RENAME pennykoin.png)


	SET(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
	SET(CPACK_PACKAGE_ICON src/images/cryptonote.png)
	
    if (NOT PACKRPM)
        set(CPACK_GENERATOR DEB)
        set(CPACK_DEBIAN_PACKAGE_NAME ${CPACK_PACKAGE_NAME})
        set(CPACK_SYSTEM_NAME amd64)
        set(CPACK_DEBIAN_PACKAGE_MAINTAINER "${CPACK_PACKAGE_VENDOR} <xrydev@gmail.com>")
        set(CPACK_DEBIAN_PACKAGE_SECTION Office)
        set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
        set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "QTPenny is the GUI wallet for pennykoin cryptocurrency.")
set(CPACK_PACKAGE_FILE_NAME "${CPACK_DEBIAN_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}-${VERSION_RELEASE}.${CPACK_SYSTEM_NAME}")
	else ()
        set(CPACK_GENERATOR RPM)
        set(CPACK_SYSTEM_NAME x86_64)
        set(CPACK_RPM_PACKAGE_RELEASE ${VERSION_RELEASE})
        set(CPACK_RPM_PACKAGE_LICENSE "GPL3")
        set(CPACK_RPM_PACKAGE_GROUP Office)
        set(CPACK_RPM_PACKAGE_REQUIRES "qt5-qtbase >= 5.3.2, qt5-qtbase-gui >= 5.3.2")
        set(CPACK_RPM_PACKAGE_SUMMARY "Pennykoin GUI wallet")
        set(CPACK_RPM_PACKAGE_DESCRIPTION "QTPenny is the GUI wallet for pennykoin cryptocurrency.")
set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}-${VERSION_RELEASE}.${CPACK_SYSTEM_NAME}")
    endif ()
elseif (WIN32)
  install(TARGETS ${PROJECT_NAME} RUNTIME DESTINATION .)
  set(CPACK_GENERATOR ZIP)
endif (APPLE)

include(CPack)
