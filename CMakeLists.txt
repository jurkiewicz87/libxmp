set(XMP_PUBLIC_INCLUDE_DIR src/public/include)
set(XMP_COMM_DIR           src/source)
set(XMP_CORE_DIR           src/source/XMPCore/source)
set(XMP_FILES_DIR          src/source/XMPFiles/source)
set(XMP_THIRDPARTY_DIR     src/third-party)

include_directories(SYSTEM src)
include_directories(SYSTEM ${XMP_COMM_DIR})
include_directories(SYSTEM src/source/XMPFilesPlugins/api/source)
include_directories(SYSTEM src/build)
include_directories(SYSTEM ${XMP_THIRDPARTY_DIR}/MD5)
include_directories(SYSTEM ${XMP_FILES_DIR})
include_directories(SYSTEM ${XMP_FILES_DIR}/FormatSupport/)

if(APPLE)
  set(XMP_ENV "MAC_ENV")
else()
  set(XMP_ENV "WIN_ENV")
endif()


if(WIN32)
  set(EXPAT_ROOT_DIR ${XMP_THIRDPARTY_DIR}/expat/lib)
  set(XMP_EXPAT "HAVE_EXPAT_CONFIG_H")
  add_definitions(-DCOMPILED_FROM_DSP)
  add_definitions(-DUNICODE)
  add_definitions(-DXML_STATIC)
  set(EXPAT_SRC
    ${EXPAT_ROOT_DIR}/amigaconfig.h
    ${EXPAT_ROOT_DIR}/ascii.h
    ${EXPAT_ROOT_DIR}/asciitab.h
    ${EXPAT_ROOT_DIR}/expat.h
    ${EXPAT_ROOT_DIR}/expat_external.h
    ${EXPAT_ROOT_DIR}/iasciitab.h
    ${EXPAT_ROOT_DIR}/internal.h
    ${EXPAT_ROOT_DIR}/latin1tab.h
    ${EXPAT_ROOT_DIR}/macconfig.h
    ${EXPAT_ROOT_DIR}/nametab.h
    ${EXPAT_ROOT_DIR}/utf8tab.h
    ${EXPAT_ROOT_DIR}/winconfig.h
    ${EXPAT_ROOT_DIR}/xmlparse.c
    ${EXPAT_ROOT_DIR}/xmlrole.c
    ${EXPAT_ROOT_DIR}/xmlrole.h
    ${EXPAT_ROOT_DIR}/xmltok.c
    ${EXPAT_ROOT_DIR}/xmltok.h
    ${EXPAT_ROOT_DIR}/xmltok_impl.c
    ${EXPAT_ROOT_DIR}/xmltok_impl.h
    ${EXPAT_ROOT_DIR}/xmltok_ns.c
   )
   source_group(expat FILES ${EXPAT_SRC})
  include_directories(SYSTEM ${EXPAT_ROOT_DIR})
else(WIN32)
  # Simply using find_library() would find the x86 only expat from vcpkg,
  # which is not suitable when building for arm64. We prefer to rely on the system expat
  # rather than adding expat to our explicitly vcpkg dependencies.
  set(EXPAT_LIB expat CACHE INTERNAL "")
endif()

set(ZUID_SRC
  ${XMP_THIRDPARTY_DIR}/zuid/interfaces/MD5.h
  ${XMP_THIRDPARTY_DIR}/zuid/interfaces/MD5.cpp
)
source_group(zuid FILES ${ZUID_SRC})

set(LIB_XMP_COMMON_SRC
  ${XMP_COMM_DIR}/Endian.h
  ${XMP_COMM_DIR}/EndianUtils.hpp
  ${XMP_COMM_DIR}/ExpatAdapter.hpp
  ${XMP_COMM_DIR}/Host_IO.hpp
  ${XMP_COMM_DIR}/IOUtils.cpp
  ${XMP_COMM_DIR}/IOUtils.hpp
  ${XMP_COMM_DIR}/PerfUtils.cpp
  ${XMP_COMM_DIR}/PerfUtils.hpp
  ${XMP_COMM_DIR}/UnicodeConversions.cpp
  ${XMP_COMM_DIR}/UnicodeConversions.hpp
  ${XMP_COMM_DIR}/UnicodeInlines.incl_cpp
  ${XMP_COMM_DIR}/XIO.cpp
  ${XMP_COMM_DIR}/XIO.hpp
  ${XMP_COMM_DIR}/XML_Node.cpp
  ${XMP_COMM_DIR}/XMLParserAdapter.hpp
  ${XMP_COMM_DIR}/XMP_LibUtils.cpp
  ${XMP_COMM_DIR}/XMP_LibUtils.hpp
  ${XMP_COMM_DIR}/XMP_ProgressTracker.cpp
  ${XMP_COMM_DIR}/XMP_ProgressTracker.hpp
  ${XMP_COMM_DIR}/XMPFiles_IO.cpp
  ${XMP_COMM_DIR}/XMPFiles_IO.hpp
)
if(WIN32)
  list(APPEND LIB_XMP_COMMON_SRC ${XMP_COMM_DIR}/Host_IO-Win.cpp)
else()
  list(APPEND LIB_XMP_COMMON_SRC ${XMP_COMM_DIR}/Host_IO-POSIX.cpp)
endif()
source_group(XmpCommon FILES ${LIB_XMP_COMMON_SRC})

set(LIB_XMP_CORE_SRC
  ${XMP_CORE_DIR}/ExpatAdapter.cpp
  ${XMP_CORE_DIR}/ParseRDF.cpp
  ${XMP_CORE_DIR}/WXMPIterator.cpp
  ${XMP_CORE_DIR}/WXMPMeta.cpp
  ${XMP_CORE_DIR}/WXMPUtils.cpp
  ${XMP_CORE_DIR}/XMPCore_Impl.cpp
  ${XMP_CORE_DIR}/XMPCore_Impl.hpp
  ${XMP_CORE_DIR}/XMPIterator.cpp
  ${XMP_CORE_DIR}/XMPIterator.hpp
  ${XMP_CORE_DIR}/XMPMeta.cpp
  ${XMP_CORE_DIR}/XMPMeta.hpp
  ${XMP_CORE_DIR}/XMPMeta-GetSet.cpp
  ${XMP_CORE_DIR}/XMPMeta-Parse.cpp
  ${XMP_CORE_DIR}/XMPMeta-Serialize.cpp
  ${XMP_CORE_DIR}/XMPUtils.cpp
  ${XMP_CORE_DIR}/XMPUtils.hpp
  ${XMP_CORE_DIR}/XMPUtils-FileInfo.cpp
)
source_group(XmpCore FILES ${LIB_XMP_CORE_SRC})

set(LIB_XMP_FILE_SRC
  ${XMP_FILES_DIR}/HandlerRegistry.cpp
  ${XMP_FILES_DIR}/HandlerRegistry.h
  ${XMP_FILES_DIR}/WXMPFiles.cpp
  ${XMP_FILES_DIR}/XMPFiles.cpp
  ${XMP_FILES_DIR}/XMPFiles.hpp
  ${XMP_FILES_DIR}/XMPFiles_Impl.cpp
  ${XMP_FILES_DIR}/XMPFiles_Impl.hpp
)
source_group(XmpFiles FILES ${LIB_XMP_FILE_SRC})

set(LIB_XMP_FILE_HANDLERS_SRC
  ${XMP_FILES_DIR}/FileHandlers/AIFF_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/AIFF_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/ASF_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/ASF_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/AVCHD_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/AVCHD_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/Basic_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/Basic_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/FLV_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/FLV_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/InDesign_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/InDesign_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/JPEG_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/JPEG_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/MP3_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/MP3_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/MPEG2_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/MPEG2_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/MPEG4_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/MPEG4_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/P2_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/P2_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/PNG_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/PNG_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/PostScript_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/PostScript_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/PSD_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/PSD_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/RIFF_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/RIFF_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/Scanner_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/Scanner_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/SonyHDV_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/SonyHDV_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/SWF_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/SWF_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/TIFF_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/TIFF_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/Trivial_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/Trivial_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/UCF_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/UCF_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/WAVE_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/WAVE_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/XDCAM_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/XDCAM_Handler.hpp
  ${XMP_FILES_DIR}/FileHandlers/XDCAMEX_Handler.cpp
  ${XMP_FILES_DIR}/FileHandlers/XDCAMEX_Handler.hpp
)
source_group(XmpFiles\\FileHandlers FILES ${LIB_XMP_FILE_HANDLERS_SRC})

set(LIB_XMP_FORMAT_SUPPORT_SRC
  ${XMP_FILES_DIR}/FormatSupport/AIFF/AIFFBehavior.cpp
  ${XMP_FILES_DIR}/FormatSupport/AIFF/AIFFBehavior.h
  ${XMP_FILES_DIR}/FormatSupport/AIFF/AIFFMetadata.cpp
  ${XMP_FILES_DIR}/FormatSupport/AIFF/AIFFMetadata.h
  ${XMP_FILES_DIR}/FormatSupport/AIFF/AIFFReconcile.cpp
  ${XMP_FILES_DIR}/FormatSupport/AIFF/AIFFReconcile.h
  ${XMP_FILES_DIR}/FormatSupport/ASF_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/ASF_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/ID3_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/ID3_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/IFF/Chunk.cpp
  ${XMP_FILES_DIR}/FormatSupport/IFF/Chunk.h
  ${XMP_FILES_DIR}/FormatSupport/IFF/ChunkController.cpp
  ${XMP_FILES_DIR}/FormatSupport/IFF/ChunkController.h
  ${XMP_FILES_DIR}/FormatSupport/IFF/ChunkPath.cpp
  ${XMP_FILES_DIR}/FormatSupport/IFF/ChunkPath.h
  ${XMP_FILES_DIR}/FormatSupport/IFF/IChunkBehavior.cpp
  ${XMP_FILES_DIR}/FormatSupport/IFF/IChunkBehavior.h
  ${XMP_FILES_DIR}/FormatSupport/IFF/IChunkContainer.h
  ${XMP_FILES_DIR}/FormatSupport/IFF/IChunkData.h
  ${XMP_FILES_DIR}/FormatSupport/IPTC_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/IPTC_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/ISOBaseMedia_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/ISOBaseMedia_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/MacScriptExtracts.h
  ${XMP_FILES_DIR}/FormatSupport/MOOV_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/MOOV_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/PackageFormat_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/PackageFormat_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/PNG_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/PNG_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/PostScript_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/PostScript_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/PSIR_FileWriter.cpp
  ${XMP_FILES_DIR}/FormatSupport/PSIR_MemoryReader.cpp
  ${XMP_FILES_DIR}/FormatSupport/PSIR_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/QuickTime_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/QuickTime_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/Reconcile_Impl.cpp
  ${XMP_FILES_DIR}/FormatSupport/Reconcile_Impl.hpp
  ${XMP_FILES_DIR}/FormatSupport/ReconcileIPTC.cpp
  ${XMP_FILES_DIR}/FormatSupport/ReconcileLegacy.cpp
  ${XMP_FILES_DIR}/FormatSupport/ReconcileLegacy.hpp
  ${XMP_FILES_DIR}/FormatSupport/ReconcileTIFF.cpp
  ${XMP_FILES_DIR}/FormatSupport/RIFF.cpp
  ${XMP_FILES_DIR}/FormatSupport/RIFF.hpp
  ${XMP_FILES_DIR}/FormatSupport/RIFF_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/RIFF_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/SWF_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/SWF_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/TIFF_FileWriter.cpp
  ${XMP_FILES_DIR}/FormatSupport/TIFF_MemoryReader.cpp
  ${XMP_FILES_DIR}/FormatSupport/TIFF_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/TIFF_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/WAVE/BEXTMetadata.cpp
  ${XMP_FILES_DIR}/FormatSupport/WAVE/BEXTMetadata.h
  ${XMP_FILES_DIR}/FormatSupport/WAVE/CartMetadata.cpp
  ${XMP_FILES_DIR}/FormatSupport/WAVE/CartMetadata.h
  ${XMP_FILES_DIR}/FormatSupport/WAVE/Cr8rMetadata.cpp
  ${XMP_FILES_DIR}/FormatSupport/WAVE/Cr8rMetadata.h
  ${XMP_FILES_DIR}/FormatSupport/WAVE/DISPMetadata.cpp
  ${XMP_FILES_DIR}/FormatSupport/WAVE/DISPMetadata.h
  ${XMP_FILES_DIR}/FormatSupport/WAVE/INFOMetadata.cpp
  ${XMP_FILES_DIR}/FormatSupport/WAVE/INFOMetadata.h
  ${XMP_FILES_DIR}/FormatSupport/WAVE/PrmLMetadata.cpp
  ${XMP_FILES_DIR}/FormatSupport/WAVE/PrmLMetadata.h
  ${XMP_FILES_DIR}/FormatSupport/WAVE/WAVEBehavior.cpp
  ${XMP_FILES_DIR}/FormatSupport/WAVE/WAVEBehavior.h
  ${XMP_FILES_DIR}/FormatSupport/WAVE/WAVEReconcile.cpp
  ${XMP_FILES_DIR}/FormatSupport/WAVE/WAVEReconcile.h
  ${XMP_FILES_DIR}/FormatSupport/XDCAM_Support.cpp
  ${XMP_FILES_DIR}/FormatSupport/XDCAM_Support.hpp
  ${XMP_FILES_DIR}/FormatSupport/XMPScanner.cpp
  ${XMP_FILES_DIR}/FormatSupport/XMPScanner.hpp
)
source_group(XmpFiles\\FormatSupport FILES ${LIB_XMP_FORMAT_SUPPORT_SRC})

set(LIB_XMP_NATIVE_METADATA_SUPPORT_SRC
  ${XMP_FILES_DIR}/NativeMetadataSupport/IMetadata.cpp
  ${XMP_FILES_DIR}/NativeMetadataSupport/IMetadata.h
  ${XMP_FILES_DIR}/NativeMetadataSupport/IReconcile.cpp
  ${XMP_FILES_DIR}/NativeMetadataSupport/IReconcile.h
  ${XMP_FILES_DIR}/NativeMetadataSupport/MetadataSet.cpp
  ${XMP_FILES_DIR}/NativeMetadataSupport/MetadataSet.h
  ${XMP_FILES_DIR}/NativeMetadataSupport/ValueObject.h
)
source_group(XmpFiles\\NativeMetadataSupport FILES ${LIB_XMP_NATIVE_METADATA_SUPPORT_SRC})

set(LIB_XMP_PLUGIN_HANDLER_SRC
  ${XMP_FILES_DIR}/PluginHandler/FileHandler.h
  ${XMP_FILES_DIR}/PluginHandler/FileHandlerInstance.cpp
  ${XMP_FILES_DIR}/PluginHandler/FileHandlerInstance.h
  ${XMP_FILES_DIR}/PluginHandler/HostAPIImpl.cpp
  ${XMP_FILES_DIR}/PluginHandler/Module.cpp
  ${XMP_FILES_DIR}/PluginHandler/Module.h
  ${XMP_FILES_DIR}/PluginHandler/ModuleUtils.h
  ${XMP_FILES_DIR}/PluginHandler/PluginManager.cpp
  ${XMP_FILES_DIR}/PluginHandler/PluginManager.h
  ${XMP_FILES_DIR}/PluginHandler/XMPAtoms.cpp
  ${XMP_FILES_DIR}/PluginHandler/XMPAtoms.h
)
if(WIN32)
  list(APPEND LIB_XMP_PLUGIN_HANDLER_SRC ${XMP_FILES_DIR}/PluginHandler/OS_Utils_WIN.cpp)
else()
  list(APPEND LIB_XMP_PLUGIN_HANDLER_SRC ${XMP_FILES_DIR}/PluginHandler/OS_Utils_Mac.cpp)
endif()
source_group(XmpFiles\\PluginHandler FILES ${LIB_XMP_PLUGIN_HANDLER_SRC})

set(LIB_XMP_SRC
  ${LIB_XMP_COMMON_SRC}
  ${LIB_XMP_CORE_SRC}
  ${LIB_XMP_FILE_SRC}
  ${LIB_XMP_FILE_HANDLERS_SRC}
  ${LIB_XMP_FORMAT_SUPPORT_SRC}
  ${LIB_XMP_NATIVE_METADATA_SUPPORT_SRC}
  ${LIB_XMP_PLUGIN_HANDLER_SRC}
  ${EXPAT_SRC} # Expat: included in xmp (on mac use the version from the system)
  ${ZUID_SRC}
 )
add_library(libxmp STATIC ${LIB_XMP_SRC})
target_compile_definitions(libxmp PRIVATE ${XMP_ENV} ${XMP_EXPAT} -DXMP_StaticBuild=1)
target_link_libraries(libxmp ${EXPAT_LIB} ${ZLIB_LIB} ${VCPKG_JPG})
target_include_directories(libxmp
 SYSTEM PUBLIC ${XMP_PUBLIC_INCLUDE_DIR}
 SYSTEM INTERFACE ${XMP_COMM_DIR})
set_property(TARGET libxmp PROPERTY FOLDER "Externals/")
